# androidsdk
Build gradle

Step 1. Add the JitPack repository to your build file
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
}

Step 2. Add the dependency
dependencies {
		compile 'com.github.momodevelopment:androidsdk:v1.0.0'
}