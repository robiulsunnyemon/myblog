---
title: Stripe Integration in Flutter
layout: default
---


# **রবিউল সানি ইমন**  
### সফটওয়্যার ডেভেলপার | ফ্লাটার, ডার্ট, পাইথন ও Django এক্স-পার্টিজ 

---

## **👤 ব্যক্তিগত তথ্য**  
- **নাম:** রবিউল সানি ইমন (Robiul Sunny Emon)  
- **পেশা:** সফটওয়্যার ডেভেলপার  
- **বিশেষায়িত ক্ষেত্র:**  
  - মোবাইল অ্যাপ ডেভেলপমেন্ট (**Flutter & Dart**)  
  - ব্যাকেন্ড ডেভেলপমেন্ট (**Python & Django**)  
  - ক্রস-প্ল্যাটফর্ম সলিউশন  
- **যোগাযোগ:**  
  - ইমেইল: [robiulsunyemon@gmail.com]()  
  - GitHub: [github.com/robiulsunnyemon]()  
  - LinkedIn: [linkedin.com/in/robiulsunnyemon]()  

---

## **💼 প্রযুক্তিগত দক্ষতা**  

### **📱 মোবাইল ডেভেলপমেন্ট (Flutter & Dart)**  
✔ **ফ্লাটার এক্সপার্ট:**  
- স্টেট ম্যানেজমেন্ট (**GetX, Provider, BLoC, Riverpod**)  
- Firebase ইন্টিগ্রেশন (**Auth, Firestore, Cloud Functions**)  
- API কানেক্টিভিটি (**REST, GraphQL, WebSockets**)  
- UI/UX ডিজাইন (**Material 3, Custom Animations**)  
- CI/CD (**Flutter Build, App Store & Play Store Deployment**)  

✔ **ডার্ট প্রোগ্রামিং:**  
- অ্যাসিনক্রোনাস প্রোগ্রামিং (**Future, Stream, Isolates**)  
- ডিজাইন প্যাটার্ন (**MVC, MVVM, Clean Architecture**)  

### **🐍 পাইথন ও Django (ব্যাকেন্ড ডেভেলপমেন্ট)**  
✔ **Django এক্সপার্ট:**  
- Django REST Framework (**API Development**)  
- ডাটাবেস ম্যানেজমেন্ট (**PostgreSQL, MySQL, SQLite**)  
- অথেন্টিকেশন (**JWT, OAuth, Session-Based Auth**)  
- Celery (**Background Tasks & Async Processing**)  

✔ **পাইথন স্ক্রিপ্টিং:**  
- ডাটা অ্যানালাইসিস (**Pandas, NumPy**)  
- অটোমেশন (**Selenium, BeautifulSoup**)  

### **🛠 অন্যান্য টুলস ও টেকনোলজি**  
- **ভার্সন কন্ট্রোল:** Git, GitHub, GitLab  
- **ডাটাবেস:** Firebase, Supabase, Hive  
- **ডেভঅপ্স:** Docker, GitHub Actions  
- **ডিজাইন:** Figma, Adobe XD  

---

## **🚀 প্রজেক্ট ও অর্জন**  

### **ফ্লাটার প্রজেক্টস**  
1. **E-Commerce App**  
   - GetX, Firebase, Stripe Payment  
   - Features: Cart, Order Tracking, Push Notifications  

2. **Health & Fitness Tracker**  
   - BLoC Pattern, SQLite, Google Fit API  
   - Real-time Health Data Monitoring  

3. **Social Media Dashboard**  
   - GraphQL API, Riverpod, Hive DB  

### **Django প্রজেক্টস**  
1. **Inventory Management System**  
   - Django REST Framework, JWT Auth, PostgreSQL  

2. **Automated Blog Platform**  
   - Web Scraping, Celery, Redis  

---

## **📚 লার্নিং রিসোর্সেস**  
- **ফ্লাটার:** [Flutter Docs](https://flutter.dev/docs), [GetX Documentation](https://pub.dev/packages/get)  
- **ডjango:** [Django Official Docs](https://docs.djangoproject.com/)  
- **পাইথন:** [Real Python](https://realpython.com/)  

---

## **📈 ভবিষ্যৎ লক্ষ্য**  
- **AI-ইন্টিগ্রেটেড অ্যাপ ডেভেলপমেন্ট**  
- **মাইক্রোসার্ভিস আর্কিটেকচার এক্সপ্লোরেশন**  
- **ওপেন-সোর্স কন্ট্রিবিউশন**  

---

### **📝 শেষ কথা**  
"কোডিং শুধু পেশা নয়, আমার আবেগ। নতুন টেকনোলজি শেখা এবং সমস্যা সমাধান করাই আমার মূল লক্ষ্য।"  

🔗 **আমার সাথে যুক্ত থাকুন:** [Portfolio](https://github.com/robiulsunnyemon?tab=repositories) | [GitHub](https://github.com/robiulsunnyemon/) | [LinkedIn](#)  

--- 

*© 2024 - রবিউল সানি ইমন* 






























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

[Next](post/flutter.md)   
[Getx_Cli](post/getx_cli.md)
