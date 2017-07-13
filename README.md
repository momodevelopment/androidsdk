# MoMo Android SDK

At a minimum, this SDK is designed to work with Android SDK 14.


## Installation

To use the MoMo Android SDK, add the compile dependency with the latest version of the MoMo SDK.

### Gradle

Step 1. Import SDK
Add the JitPack repository to your `build.gradle`:
```
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

Add the dependency:
```
dependencies {
    compile 'com.github.momodevelopment:androidsdk:v1.0.0'
}
```
 
Step 2. Config AndroidMainfest
```
<uses-permission android:name="android.permission.INTERNET" />
```
Step 3. Build Layout
Confirm order Activity
```
import vn.momo.momo_partner.AppMoMoLib;
import vn.momo.momo_partner.MoMoParameterNameMap;

void onCreate(Bundle savedInstanceState) 
        AppMoMoLib.getInstance().setEnvironment(AppMoMoLib.ENVIRONMENT.DEVELOPMENT); // AppMoMoLib.ENVIRONMENT.PRODUCTION
```
Display MoMo button label language: English = "MoMo e-wallet", "Vietnamese = Ví MoMo"

Step 4. app MoMo app & request payment
```
//Get token through MoMo app 
void requestToken() {
        AppMoMoLib.getInstance().setAction(AppMoMoLib.ACTION.PAYMENT);
        AppMoMoLib.getInstance().setActionType(AppMoMoLib.ACTION_TYPE.GET_TOKEN);
        if (edAmount.getText().toString() != null && edAmount.getText().toString().trim().length() != 0)
            amount = edAmount.getText().toString().trim();

        Map<String, Object> eventValue = new HashMap<>();
        //client Required
        eventValue.put(MoMoParameterNamePayment.MERCHANT_NAME, merchantName); //example:  CGV Cinemas
        eventValue.put(MoMoParameterNamePayment.MERCHANT_CODE, merchantCode); //example:  SCB01
        eventValue.put(MoMoParameterNamePayment.AMOUNT, amount);              //example:  99000
        eventValue.put(MoMoParameterNamePayment.DESCRIPTION, description);    //example:  OrderId : 123456
        //client Optional
        eventValue.put(MoMoParameterNamePayment.FEE, fee);                    //display only, example: 0
        eventValue.put(MoMoParameterNamePayment.MERCHANT_NAME_LABEL, merchantNameLabel); //example: Company 
        eventValue.put(MoMoParameterNamePayment.USER_NAME, useremail_or_username);       //example: user0123456@gmail.com 
        //client info custom parameter
        JSONObject objExtra = new JSONObject();
        try {
            objExtra.put("key1", "value1"); //example: promocode = 12345
        } catch (JSONException e) {
            e.printStackTrace();
        }
        eventValue.put(MoMoParameterNamePayment.EXTRA, objExtra);
        AppMoMoLib.getInstance().requestMoMoCallBack(this, eventValue);
    } 
//Get token callback from MoMo app an submit to server side 
void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if(requestCode == AppMoMoLib.getInstance().REQUEST_CODE_MOMO && resultCode == -1) {
            if(data != null) {
                if(data.getIntExtra("status", -1) == 0) {
                    //TOKEN IS AVAILABLE 
                    tvMessage.setText("message: " + "Get token " + data.getStringExtra("message"));
                    String token = data.getStringExtra("data"); //Token response 
                    String phoneNumber = data.getStringExtra("phonenumber");
                    if(token != null && !token.equals("")) {
                        // TODO: send phoneNumber & token to your server side to process payment with MoMo server
                        // IF Momo topup success, continue to process your order
                    } else {
                        tvMessage.setText("message: " + this.getString(R.string.not_receive_info));
                    }
                } else if(data.getIntExtra("status", -1) == 1) {
                    //TOKEN FAIL 
                    String message = data.getStringExtra("message") != null?data.getStringExtra("message"):"Thất bại";
                    tvMessage.setText("message: " + message);
                } else if(data.getIntExtra("status", -1) == 2) {
                    //TOKEN FAIL
                    tvMessage.setText("message: " + this.getString(R.string.not_receive_info));
                } else {
                    //TOKEN FAIL
                    tvMessage.setText("message: " + this.getString(R.string.not_receive_info));
                }
            } else {
                tvMessage.setText("message: " + this.getString(R.string.not_receive_info));
            }
        } else {
            tvMessage.setText("message: " + this.getString(R.string.not_receive_info_err));
        }
    }
```


