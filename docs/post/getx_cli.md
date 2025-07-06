# GetX CLI ডকুমেন্টেশন  

## পরিচিতি  
GetX CLI হলো Flutter-এর GetX স্টেট ম্যানেজমেন্ট লাইব্রেরির জন্য একটি শক্তিশালী কমান্ড-লাইন টুল, যা আপনার প্রজেক্টের বয়লারপ্লেট কোড জেনারেট করতে সাহায্য করে। এই ডকুমেন্টেশনে GetX CLI-এর ইনস্টলেশন এবং ব্যবহার সম্পর্কে বিস্তারিত আলোচনা করা হবে।  

## ইনস্টলেশন  

### প্রয়োজনীয় জিনিস  
- Flutter SDK ইন্সটল করা থাকতে হবে  
- Dart SDK ইন্সটল করা থাকতে হবে  

### ইনস্টলেশন স্টেপ  
1. GetX CLI প্যাকেজটি অ্যাক্টিভেট করুন:  
```bash
dart pub global activate get_cli
```  
2. সিস্টেম ভেরিয়েবল এড করুন:
```
%USERPROFILE%\AppData\Local\Pub\Cache\bin
```

2. ইনস্টলেশন যাচাই করুন:  
```bash
get --version
```  

## বেসিক কমান্ড  

### নতুন প্রজেক্ট তৈরি  
```bash
get create project:project_name
```  

### নতুন পেজ জেনারেট করুন  
```bash
get create page:home
```  

### কন্ট্রোলার জেনারেট করুন  
```bash
get create controller:home
```  

### ভিউ জেনারেট করুন  
```bash
get create view:home
```  

### উইজেট জেনারেট করুন  
```bash
get create widget:custom_button
```  

## অ্যাডভান্সড কমান্ড  

### সম্পূর্ণ ফিচার তৈরি (পেজ + কন্ট্রোলার + ভিউ + বাইন্ডিং)  
```bash
get create page:profile
```  

### মডেল জেনারেট করুন  
```bash
get create model:user
```  

### প্রোভাইডার জেনারেট করুন  
```bash
get create provider:user
```  

### রিপোজিটরি জেনারেট করুন  
```bash
get create repository:user
```  

## অপশনসমূহ  

### --skip-routes  
রাউট ফাইলে রাউট অ্যাড করা স্কিপ করুন  
```bash
get create page:settings --skip-routes
```  

### --skip-test  
টেস্ট ফাইল তৈরি করা স্কিপ করুন  
```bash
get create page:settings --skip-test
```  

### --on  
ফোল্ডার স্ট্রাকচার নির্দিষ্ট করুন  
```bash
get create page:admin/dashboard --on modules
```  

## কনফিগারেশন  

### কনফিগ ফাইল ইনিশিয়ালাইজ করুন  
```bash
get init
```  

এটি `get_cli.yaml` ফাইল তৈরি করবে, যেখানে আপনি ডিফল্ট সেটিংস কনফিগার করতে পারবেন, যেমন:  
- প্রজেক্ট স্ট্রাকচার  
- স্টেট ম্যানেজমেন্ট অ্যাপ্রোচ  
- ফাইল নেমিং কনভেনশন  
- ইত্যাদি  

## সাধারণ ওয়ার্কফ্লো  

### একটি সম্পূর্ণ ফিচার তৈরি  
1. বাইন্ডিং সহ পেজ জেনারেট করুন:  
```bash
get create page:product_detail --with-binding
```  

2. মডেল জেনারেট করুন:  
```bash
get create model:product
```  

3. প্রোভাইডার জেনারেট করুন:  
```bash
get create provider:product
```  

4. রিপোজিটরি জেনারেট করুন:  
```bash
get create repository:product
```  

## টিপস ও ট্রিকস  

- `get help` ব্যবহার করে সব কমান্ড দেখুন  
- ফ্ল্যাগ ব্যবহার করে কাস্টমাইজড জেনারেশন করুন  
- CLI অটোমেটিক্যালি আপনার রাউট ফাইল আপডেট করে  
- `.get_cli` ফোল্ডারে কাস্টম টেমপ্লেট রেখে ডিফল্ট টেমপ্লেট ওভাররাইড করতে পারেন  

## উপসংহার  

GetX CLI ফ্লাটার ডেভেলপমেন্টকে দ্রুততর করে বয়লারপ্লেট কোড অটোমেটিক জেনারেট করে। এই কমান্ডগুলো ব্যবহার করে আপনি সহজেই সম্পূর্ণ ফিচার তৈরি করতে পারবেন এবং প্রজেক্টে কনসিস্টেন্ট কোড স্ট্রাকচার মেইন্টেইন করতে পারবেন।  

আরও অ্যাডভান্সড ব্যবহারের জন্য অফিসিয়াল GetX CLI ডকুমেন্টেশন দেখুন:  
[https://pub.dev/packages/get_cli](https://pub.dev/packages/get_cli)
