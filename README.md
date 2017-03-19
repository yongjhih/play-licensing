Client library for the Android Market licensing server.

See the licensing documentation at http://developer.android.com/guide/publishing/licensing.html

## Usage

```java
    // ref. https://developer.android.com/google/play/licensing/adding-licensing.html
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // CHANGE ME
        final byte[] SALT = new byte[] {
             -46, 65, 30, -128, -103, -57, 74, -64, 51, 88, -95,
             -45, 77, -117, -36, -113, -11, 32, -64, 89
             };

        mLicenseChecker = new LicenseChecker(getApplicationContext(),
                                             new ServerManagedPolicy(getApplicationContext(),
                                                                     new AESObfuscator(SALT,
                                                                                       getPackageName(),
                                                                                       Settings.Secure.getString(context.contentResolver, Settings.Secure.ANDROID_ID))),
                                             getResources().getString(R.string.google_license));

        mLicenseChecker.checkAccess(new LicenseCheckerCallback() {
            @Override
            public void allow(int reason) {
                if (activity.isFinishing()) return;
            }

            @Override
            public void dontAllow(int reason) {
                if (activity.isFinishing()) return;

                if (reason != Policy.RETRY) {
                    AlertDialog.Builder(context)
                        .setTitle("License Check")
                        .setMessage("Please install latest version from official site")
                        .setCancelable(false)
                        .show()
                }
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mLicenseChecker.onDestroy();
    }
```

```xml
    <string name="google_license">FFFFF==</string>
```

## Installation

```gradle
repositories {
    jcenter()
    maven { url "https://jitpack.io" }
}

dependencies {
    compile 'com.github.google:play-licensing:-SNAPSHOT'
}
```

```xml
    <uses-permission android:name="com.android.vending.CHECK_LICENSE" />
```

## License

Apache 2.0
