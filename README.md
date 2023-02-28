---
description: Here just some my study notes for Android SELinux
---

# Android SELinux Introduction

Google had good website for the Android SELinux https://source.android.com/docs/security/features/selinux. **But something still not clear and mess organized**. Here just list some work notes and some questions & answers for my reading & Study notes for better understand.

## SELinux in Linux

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Concepts

{% embed url="https://source.android.com/docs/security/features/selinux/concepts" %}

| Name             | Explain                                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Security Context | username:role:type:MLS security range                                                                                                   |
| Security Policy  | Used by kernel security server to allow or disallow access to kernel objects at runtime. It is composited by statements and rules.      |
| Access Vector    | SELinux permission, refer to system/sepolicy/prebuilts/api/28.0/private/access\_vectors which Define common prefixes for access vectors |

**Username** may be a group or class of user, one user may have several roles. One **role** may associated with one or more **domain** types, **type** is used to group processes in a domain or an object logic type. In Android user and security range are always set to _u_ and _s0_. The role is set to either _r_ for domains (processes) or to the built-in _object\_r_ role for objects. In Filesystem, it uses _security.selinux_ to store the security context of file objects.&#x20;

### Statement

<pre><code><strong>attribute domain;     // domain is an attribute
</strong>type adbd, domain;     //declare adbd type, and associate it with domain;
user u roles { r } level s0 range s0 - mls_systemhigh; //declare user u
role r types domain;    // accociate r with domain attribute
permissive adbd; //set adbd domain running in permissive mode
class file //declare SELinux object class of "file"

//https://selinuxproject.org/page/TypeRules
# The rule states that when a process of type acct_t creates a 
# file in the directory of type var_log_t, by default it should 
# have the type wtmp_t if allowed by the policy.
type_transition acct_t var_log_t:file wtmp_t;

type_transition vold storage_file:dir storage_stub_file; //



//allow
allow vold sdcard_type:filesystem { mount remount unmount };

//neverallow
neverallow { domain -init } kernel:security load_policy;
</code></pre>

### Access Vector

```
rule_name source_type target_type : class perm_set;
```

rule\_name are, `allow`, `dontaudit`, `auditallow`, or `neverallow.`

``

### How to setup relationship of types

attributes can be used associate types.&#x20;

```
# Associate the attribute appdomain with the type untrusted_app.
typeattribute untrusted_app, appdomain;
```

### Android doesn't use all SELinux features

Copy from [https://source.android.com/docs/security/features/selinux/concepts#specificity](https://source.android.com/docs/security/features/selinux/concepts#specificity)

Android does not use all the features provided by SELinux. When reading external documentation, keep these points in mind:

* The majority of the policies in AOSP are defined using the Kernel Policy Language. There are some exceptions for using Common Intermediate Language (CIL).
* SELinux _users_ are not used. The only user defined is [`u`](https://android.googlesource.com/platform/system/sepolicy/+/refs/heads/master/private/users). When necessary, physical users are represented using the categories field of a security context.
* SELinux _roles_ and Role-Based Access Control (RBAC) are not used. Two default roles are defined and used: [`r`](https://android.googlesource.com/platform/system/sepolicy/+/refs/heads/master/private/roles\_decl) for subjects and `object_r` for objects.
* SELinux _sensitivities_ are not used. The default `s0` sensitivity is always set.
* SELinux booleans are not used. Once the policy is built for a device, it does not depend on the state of the device. This simplifies the auditing and debugging of policies.

## Android SELinux key files

{% embed url="https://source.android.com/docs/security/features/selinux/implement#key_files" %}

{% embed url="https://source.android.com/docs/security/features/selinux/compatibility#selinux-contexts-labeling" %}



#### _file\_contexts_ &#x20;

assigns labels to files and is used by various userspace components.

* _init_ uses the `restorecon` command to look up the security context of each file in _file\_contexts_
* When building Android from source, the make\_ext4fs command also consults file\_contexts in order to set the initial contexts of files on the system & userdata
* Recovery also have file\_contexts to set correct label in system update

Android 8.0 splits to /system/etc/selinux/plat\_file\_contexts & /vendor/etc/selinux/vendor\_file\_contexts (built from BOARD\_SEPOLICY\_DIRS)

#### _genfs\_contexts_

assigns labels to filesystems, such as `proc` or `vfat` that do not support extended attributes.

#### _property\_contexts_

assigns labels to Android system properties to control what processes can set them.

* Loaded by _property\_service_ (part of _init_)
* only enforce set. no get? &#x20;
* get\_prop(radio, vendor\_rild\_libpath\_prop)

in Android 8.0 splits to /system/etc/selinux/plat\_property\_contexts & /vendor/etc/selinux/vendor\_property\_contexts (built from BOARD\_SEPOLICY\_DIRS).&#x20;

#### _service\_contexts_

assigns labels to Android binder services to control what processes can add (register) and find (lookup) a binder reference for the service

* loaded by `servicemanager` at the start along with the vendor `service_contexts  OR hwservicemanager` OR `vndservicemanager`&#x20;

splits to /system/etc/selinux/plat\_service\_contexts & /vendor/etc/selinux/vendor\_service\_contexts & /system/etc/selinux/plat\_hwservice\_contexts & /vendor/etc/selinux/vendor\_hwservice\_contexts & /vendor/etc/selinux/vndservice\_contexts



#### _seapp\_contexts_

assigns labels to app processes and `/data/data` directories. read by the `zygote` process on each app launch and by `installd` during startup.

* &#x20;_zygote_ has been extended to check the security context of its clients (implemented in the `ZygoteConnection` class) and set the security context of each app process that it forks.
* \_solated services are given the _\_isolated_ user-name string, and any other process is given the _\_app_ username string.&#x20;

```
user=system domain=system_app type=system_data_file

//user selector is defined by android user id
```

Splits to /system/etc/selinux/plat\_seapp\_contexts & /vendor/etc/selinux/vendor\_seapp\_contexts

#### _mac\_permission.xml_

assigns a `seinfo` tag to apps based on their signature and optionally their package name.

* Middleware MAC, The `seinfo` attribute introduced in the previous section is part of an SEAndroid feature called _middleware MAC (MMAC)_, which is a higher-level access control scheme, separate from the kernel-level MAC (implemented in the SELinux LSM module).
* When scanning each installed package, the system `PackageManagerService` matches its signing certificate against the contents of the _mac\_permission.xml_ file and assigns the specified `seinfo` value to the package if it finds a match. If no match is found, it assigns the _default_ `seinfo` value as specified by the `<default>` tag&#x20;

splits to /system/etc/selinux/mac\_permissions.xm & /vendor/etc/selinux/mac\_permissions.xm

#### keystore2\_key\_contexts

assigns labels to Keystore 2.0 namespaces.

## SELinux Policy files location

Android SeLinux policy [https://source.android.com/docs/security/features/selinux/build](https://source.android.com/docs/security/features/selinux/build)

{% embed url="https://source.android.com/docs/security/features/selinux/customize#android-o" %}

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

* Core policy files, those files comprise the SELinux kernel security policy and cover the upstream Android operating system [https://android.googlesource.com/platform/system/sepolicy/](https://android.googlesource.com/platform/system/sepolicy/)
* /device/manufacturer/device-name/sepolicy, will hold the OEM policies. 4 locations (platform/vendor/system\_ext/product) seplicy

```
BOARD_VENDOR_SEPOLICY_DIRS += device/samsung/tuna/sepolicy
SYSTEM_EXT_PUBLIC_SEPOLICY_DIRS += device/acme/roadrunner-sepolicy/systemext/public
SYSTEM_EXT_PRIVATE_SEPOLICY_DIRS += device/acme/roadrunner-sepolicy/systemext/private
PRODUCT_PUBLIC_SEPOLICY_DIRS += device/acme/roadrunner-sepolicy/product/public
PRODUCT_PRIVATE_SEPOLICY_DIRS += device/acme/roadrunner-sepolicy/product/private
```

### Meaning of public and private

According to [https://source.android.com/docs/security/features/selinux/build](https://source.android.com/docs/security/features/selinux/build)

**Public policy** is exported to vendor. Types and attributes become stable API, and vendor policy can refer to types and attributes in the public policy. Types are versioned according to `PLATFORM_SEPOLICY_VERSION`, and the versioned policy is included to the vendor policy. The original policy is included to each of system\_ext and product partition.&#x20;

**Private policy** contains system\_ext-only and product-only types, permissions, and attributes required for system\_ext and product partitions' functionality. Private policy is invisible to vendor, implying that these rules are internal and allowed to be modified.

system\_ext and product are allowed to export their designated public types to vendor.

## How Android Label the objects

{% embed url="https://source.android.com/docs/security/features/selinux/compatibility#object-ownership-and-labeling" %}

### `Android key domains`

`unconfineddomain`already be removed in the latest Android release.

`appdomain`  These application domains are assigned common permissions by inheriting the base `appdomain` using the `app_domain()` macro which, as defined in _app.te_, includes rules that allow the common operations all Android apps require.

`untrusted_app` (which is assigned to all non-system applications according to the assignment rules in _seapp\_contexts_ shown in [Example 12-22](https://learning-oreilly-com.sjezp01.sjlibrary.org/library/view/android-security-internals/9781457185496/ch12.html#contents\_of\_the\_seappunderscorecontexts)) extend `appdomain`

`isolated_app`, `media_app`, `platform_app`, `release_app`, and `shared_app` in version 4.4) also inherit from `appdomain` and add additional `allow` rules, either directly or by extending additional domains



### Available Controls

[https://source.android.com/docs/security/features/selinux/customize#available\_controls](https://source.android.com/docs/security/features/selinux/customize#available\_controls)

### How android label objects

I see some rules in Android to label the files...

{% embed url="https://source.android.com/docs/security/features/selinux/compatibility#object-ownership-and-labeling" %}

## SELinux policy split

Copy from [https://android.googlesource.com/platform/system/core/+/master/init/selinux.cpp](https://android.googlesource.com/platform/system/core/+/master/init/selinux.cpp)

The SEPolicy on Android comes in two variants: monolithic and split. The monolithic policy variant is for legacy non-treble devices that contain a single SEPolicy file located at /sepolicy and is directly loaded into the kernel SELinux subsystem.

The split policy is for supporting treble devices and updateable apexes:

* /system/etc/selinux (the 'plat' portion of the policy)
* /vendor/etc/selinux (the 'vendor' portion of the policy)
* /system\_ext/etc/selinux
* /product/etc/selinux
* /odm/etc/selinux
* /dev/selinux (the apex portion of policy)

## Explore Android SEPolicy by tools

in external/selinux/prebuilts/bin, we can see the selinux prebuilt tool

```
audit2allow
audit2why
sediff
sediff.py
seinfo
seinfo.py
sesearch
sesearch.py
```

### Config SELinux tool env

Android prebuild selinux tools need runing env

```
. build/envsetup.sh
lunch
//choice one 
Then you can setup the env to run the selinux tools
```

```
//Setup if you are running it in a server without sudo permission, you need
//install the python modules, please refer to following guide to install and config
//python in your personal folder
https://pages.github.nceas.ucsb.edu/NCEAS/Computing/local_install_python_on_a_server.html
https://help.dreamhost.com/hc/en-us/articles/115000702772-Installing-a-custom-version-of-Python-3
```

### Usage examples

```
//list domain
seinfo -adomain -x out/target/product/{devicename}/root/sepolicy

Type Attributes: 1
   attribute domain;
	DcxoSetCap
	EEReport
	GoogleOtaBinder
	aal
	adbd
	aee_aedv
	aee_core_forwarder
	aee_hal
	aidl_lazy_test_server
	alpsd_app

//search allow rule with zygote as source
sesearch --allow -s zygote -ds out/target/product/{devicename}/root/sepolicy
```

#### `Other Android Tools`

{% embed url="https://android.googlesource.com/platform/system/sepolicy/+/master/tools/README" %}

## How Android load the policies

the CIL files are pre-compiled at build time and placed at either `/vendor/etc/selinux/precompiled_sepolicy` or `/odm/etc/selinux/precompiled_sepolicy`, along with the sha256 hashes of the input CIL files. At runtime, `init` checks if any of the policy file has been updated by comparing the hashes. If nothing has changed, `init` loads the precompiled policy. If not, `init` compiles on the fly and uses it instead of the precompiled one.

/system contains the SEPolicy compiler, secilc, and it is used by the OpenSplitPolicy() function below to compile the SEPolicy to a temp directory and load it.

## References

* <\<android-security-internals>> Charter 12
* [https://ge0n0sis.github.io/posts/2015/12/exploring-androids-selinux-kernel-policy/](https://ge0n0sis.github.io/posts/2015/12/exploring-androids-selinux-kernel-policy/)
* h[ttps://android.googlesource.com/platform/external/selinux/+/refs/heads/master/secilc/README](https://android.googlesource.com/platform/external/selinux/+/refs/heads/master/secilc/README)
* [http://github.com/SELinuxProject/cil/wiki/](http://github.com/SELinuxProject/cil/wiki/)
* [https://deepinout.com/android-system-analysis/android-security-related/easy-to-understand-selinux.html](https://deepinout.com/android-system-analysis/android-security-related/easy-to-understand-selinux.html)
* h[ttps://android.googlesource.com/platform/system/core/+/master/init/selinux.cpp](https://android.googlesource.com/platform/system/core/+/master/init/selinux.cpp)

