# How to check the value in framework-res.apk

1. cp out/target/product/{product name}/system/framework/framework-res.apk \~/
2. aapt d --values resources framework-res.apk > framework-res\_resources.txt

