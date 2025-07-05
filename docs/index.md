---
title: Stripe Integration in Flutter
layout: default
---


## 💳📱 Flutter অ্যাপে Stripe Payment Integration করুন — খুব সহজে!

Flutter অ্যাপে Payment System লাগবে? আজ দেখিয়ে দিচ্ছি কীভাবে মাত্র ৪ ধাপে Stripe Payment Gateway ইনটিগ্রেট করা যায়! 🚀

---

### 🧩 Stripe এর dependency যোগ করুন

`pubspec.yaml` ফাইলে নিচের কোড দিন:

```yaml
flutter_stripe: ^10.0.0
http: ^0.13.5
```
### টার্মিনালে রান করুন:
```
flutter pub get
```
### 🧠 Stripe initialize করুন main.dart ফাইলে
```
void main() async {
  _setup();  
  runApp(MyApp());
}

Future<void> _setup()async{
  WidgetsFlutterBinding.ensureInitialized();
  Stripe.publishableKey=stripePublishableKey;
}

```


### Copy Keys from your stripe account
```
const String stripePublishableKey = "pk_test_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";
const String stripeSecretKey = "sk_test_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";
```

## 🛠️ Step-by-Step Stripe Integration:


###  1. 🧾 Make Payment Flow Method
  ```
   Future<void> makePayment() async {
    try {
      String? clientSecret = await _createPaymentIntent(10, "usd");
      if (clientSecret == null) return;

      // Step 1: Initialize payment sheet
      await Stripe.instance.initPaymentSheet(
        paymentSheetParameters: SetupPaymentSheetParameters(
          paymentIntentClientSecret: clientSecret,
          merchantDisplayName: "Khal",
        ),
      );

      await _processPayment();
    } catch (e) {
      print("Payment error: ${e.toString()}");
    }
  }
```
###  2. 💵 Create Payment Intent Method
```
  Future<String?> _createPaymentIntent(int amount, String currency) async {
    try {
      Map<String, dynamic> body = {
        'amount': _calculateAmount(amount),
        'currency': currency,
      };

      var response = await http.post(
        Uri.parse("https://api.stripe.com/v1/payment_intents"),
        body: body,
        headers: {
          'Authorization': 'Bearer $stripeSecretKey',
          'Content-Type': 'application/x-www-form-urlencoded',
        },
      );

      if (response.statusCode == 200) {
        final json = jsonDecode(response.body);
        return json['client_secret'];
      } else {
        print("Failed to create payment intent: ${response.body}");
        return null;
      }
    } catch (e) {
      print("Error creating PaymentIntent: ${e.toString()}");
      return null;
    }
  }
```
  ###  3. 💳  Create Process Payment Method
  ```
  Future<void> _processPayment() async {
    try {
      await Stripe.instance.presentPaymentSheet();
      print("Payment Successful");
      Get.to(()=>OrderConfirmationView());
    } catch (e) {
      print("Error presenting payment sheet: ${e.toString()}");
    }
  }
```



### 4. 🧾 Calculate Amount
```
  String _calculateAmount(int amount) {
    // Stripe expects amount in cents
    final calculated = amount * 100;
    return calculated.toString();
  }
```

## All Code Snipet
```
class StripeService {
  StripeService._();

  static final StripeService instance = StripeService._();

  Future<void> makePayment() async {
    try {
      String? clientSecret = await _createPaymentIntent(10, "usd");
      if (clientSecret == null) return;

  
      await Stripe.instance.initPaymentSheet(
        paymentSheetParameters: SetupPaymentSheetParameters(
          paymentIntentClientSecret: clientSecret,
          merchantDisplayName: "Khal",
        ),
      );


      await _processPayment();
    } catch (e) {
      print("Payment error: ${e.toString()}");
    }
  }

  Future<String?> _createPaymentIntent(int amount, String currency) async {
    try {
      Map<String, dynamic> body = {
        'amount': _calculateAmount(amount),
        'currency': currency,
      };

      var response = await http.post(
        Uri.parse("https://api.stripe.com/v1/payment_intents"),
        body: body,
        headers: {
          'Authorization': 'Bearer $stripeSecretKey',
          'Content-Type': 'application/x-www-form-urlencoded',
        },
      );

      if (response.statusCode == 200) {
        final json = jsonDecode(response.body);
        return json['client_secret'];
      } else {
        print("Failed to create payment intent: ${response.body}");
        return null;
      }
    } catch (e) {
      print("Error creating PaymentIntent: ${e.toString()}");
      return null;
    }
  }

  Future<void> _processPayment() async {
    try {
      await Stripe.instance.presentPaymentSheet();
      print("Payment Successful");
      Get.to(()=>OrderConfirmationView());
    } catch (e) {
      print("Error presenting payment sheet: ${e.toString()}");
    }
  }

  String _calculateAmount(int amount) {
    // Stripe expects amount in cents
    final calculated = amount * 100;
    return calculated.toString();
  }
}

```

### ✅ ব্যাস! Stripe ইনটিগ্রেশন শেষ! 💸
টেস্ট করার জন্য Stripe এর টেস্ট কার্ড ব্যবহার করুন:

```
কার্ড: 4242 4242 4242 4242  
```
```
তারিখ: যেকোনো ভবিষ্যৎ তারিখ (08/76) 
```
```
CVC: যেকোনো 3 ডিজিট (3333)
```

### ⚡ অতিরিক্ত টিপস:
✅ FVM ব্যবহার করলে Flutter version handle সহজ হয়

✅ Secret Key কখনো client-side এ রাখবেন না (production এ backend ব্যবহার করুন)

✅ সবসময় test mode দিয়ে development করুন
