

### **Flutter + Clean Architecture**

ধরুন, আপনি একটি রেস্টুরেন্টে ঢুকলেন। রেস্টুরেন্টটি সুন্দরভাবে সাজানো—কিচেন আলাদা, ডাইনিং এরিয়া আলাদা, ওয়েটাররা শুধু অর্ডার নেয় এবং সার্ভ করে, আর শেফ শুধু রান্না করেন। এই পুরো সিস্টেমটাই একটি **ক্লিন আর্কিটেকচার**। ফ্লাটার অ্যাপ ডেভেলপমেন্টেও আমরা এমনই একটি সুসংগঠিত কাঠামো চাই, যেখানে প্রতিটি অংশের কাজ আলাদা, একটি অংশের পরিবর্তন অন্যটিকে প্রভাবিত করবে না।

ক্লিন আর্কিটেকচারের মূল উদ্দেশ্য হল **সেপারেশন অফ কনসার্নস**। ফ্লাটারে আমরা এটি বাস্তবায়ন করি প্রধানত কয়েকটি লেয়ার বা স্তরে ভাগ করে:

1.  **প্রেজেন্টেশন লেয়ার** (UI)
2.  **ডোমেইন লেয়ার** (Business Logic)
3.  **ডাটা লেয়ার** (Data Source)


```
lib/
├── core/
│   ├── constants/
│   ├── errors/
│   ├── network/
│   ├── usecases/
│   └── utils/
├── features/
│   ├── product/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   ├── models/
│   │   │   └── repositories/
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   ├── repositories/
│   │   │   └── usecases/
│   │   └── presentation/
│   │       ├── blocs/
│   │       ├── pages/
│   │       ├── widgets/
│   │       └── widgets/
│   ├── category/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   ├── slider/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   └── home/
│       ├── domain/
│       │   └── usecases/
│       └── presentation/
│           ├── blocs/
│           ├── pages/
│           └── widgets/
├── injection_container.dart
└── main.dart

```







প্রতিটি লেয়ারের জন্য আমরা আলাদা আলাদা ফোল্ডার তৈরি করি। প্রথমে আমরা আমাদের প্রজেক্টের সবচেয়ে গুরুত্বপূর্ণ ফোল্ডার `core` এবং `data` ফোল্ডার নিয়ে আলোচনা করব।

#### **`core` ফোল্ডারঃ**

`core` ফোল্ডারকে আমরা অ্যাপের মেরুদণ্ড বলি এইখানে, সেই সব ফাইল এবং ক্লাস রাখি যা পুরো অ্যাপ জুড়ে ব্যবহৃত হয় এবং যেগুলো অ্যাপের নির্দিষ্ট কোনো ফিচারের সাথে সরাসরি সম্পর্কিত নয়। Think of it as the restaurant's "Utility Room".

**`core` ফোল্ডারে সাধারণত যা থাকেঃ**

1.  **`constants/`:** অ্যাপের সকল ধ্রুবক মান এখানে থাকে। যেমন- API URL, App Name, Error Messages ইত্যাদি।
   
    ```dart
    // core/constants/app_constants.dart
    class AppConstants {
      static const String baseUrl = 'https://api.myapp.com';
      static const String appName = 'My Clean App';
    }
    ```

3.  **`utils/`:** ছোটখাটো ইউটিলিটি ফাংশন, ভ্যালিডেশন লজিক, ক্যালকুলেশন ফাংশন ইত্যাদি এখানে থাকে।
   
    ```dart
    // core/utils/input_validator.dart
    class InputValidator {
      static String? validateEmail(String? value) {
        if (value == null || value.isEmpty) {
          return 'Please enter an email';
        }
        // ... regex validation
        return null;
      }
    }
    ```

5.  **`widgets/`:** পুরো অ্যাপ জুড়ে বারবার ব্যবহৃত কাস্টম উইজেটগুলো (যেমন- Custom Button, AppBar, Loader) এখানে রাখা হয়। এতে কোড রিইউজ হয় এবং শৃঙ্খলা বজায় থাকে।

6.  **`errors/`:** অ্যাপের বিভিন্ন এক্সেপশন এবং এরর ক্লাসগুলো ডিফাইন করা হয় এখানে।
   
    ```dart
    // core/errors/failures.dart
    abstract class Failure {
      final String message;
      Failure(this.message);
    }

    class ServerFailure extends Failure {
      ServerFailure(String message) : super(message);
    }

    class CacheFailure extends Failure {
      CacheFailure(String message) : super(message);
    }
    ```

8.  **`network/`:** HTTP ক্লায়েন্ট (Dio বা http প্যাকেজের ইনস্ট্যান্স), API কনফিগারেশন ইত্যাদি এখানে থাকে।

9.  **`theme/`:** অ্যাপের কালার, টেক্সট স্টাইল, থিম ডাটা এখানে ডিফাইন করা হয়।

#### **`data` ফোল্ডারঃ**

এখন আসি `data` ফোল্ডারে। এই ফোল্ডারটি সম্পূর্ণরূপে **ডাটা নিয়ে কাজ করে**—ডাটা কোথা থেকে আসছে (API নাকি Local Database), কীভাবে আসছে, সেটা কোথায় সেভ হচ্ছে—সবকিছুর দায়িত্ব এই লেয়ারের।

**`data` ফোল্ডারের ভিতরে সাধারণত তিনটি সাব-ফোল্ডার থাকেঃ**

1.  **`models/`**
    *   **কাজ কি?** এখানে আমরা আমাদের ডাটার ব্লুপ্রিন্ট বা স্ট্রাকচার ডিফাইন করি। API থেকে যে JSON ডাটা আসে, সেটাকে Dart Object-এ রূপান্তর করার জন্য Model Class তৈরি করা হয় এখানেই।
    ধরুন, আমাদের অ্যাপে একটি User আছে। API থেকে User-এর ডাটা JSON আকারে আসে `{ "id": 1, "name": "Robiul Suny", "email": "robiulsunyemon@gmail.com" }`। আমরা এই JSON কে Dart-এ নিতে একটি Model Class বানাবো।

        ```dart
        // data/models/user_model.dart
        class UserModel {
          final int id;
          final String name;
          final String email;

          UserModel({required this.id, required this.name, required this.email});

          // JSON থেকে Model বানানোর ফ্যাক্টরি কনস্ট্রাক্টর
          factory UserModel.fromJson(Map<String, dynamic> json) {
            return UserModel(
              id: json['id'],
              name: json['name'],
              email: json['email'],
            );
          }

          // Model থেকে JSON বানানোর মেথড
          Map<String, dynamic> toJson() => {
            'id': id,
            'name': name,
            'email': email,
          };
        }
        ```

2.  **`datasources/`**
    *   **কাজ কি?** ডাটার আসল সোর্স (API, Local Database like Hive/SharedPreferences) এর সাথে সরাসরি যোগাযোগ করে এই ক্লাসগুলো। এগুলোকে ডাটা পাওয়ার "দরজা" হিসেবে ভাবতে পারেন। এখানে `UserRemoteDataSource` ক্লাসটি শুধুমাত্র API কে কল করবে এবং Raw JSON Data নিয়ে আসবে।

        ```dart
        // data/datasources/user_remote_data_source.dart
        abstract class UserRemoteDataSource {
          Future<UserModel> getUser(int userId);
        }

        class UserRemoteDataSourceImpl implements UserRemoteDataSource {
          final Dio dio;

          UserRemoteDataSourceImpl({required this.dio});

          @override
          Future<UserModel> getUser(int userId) async {
            final response = await dio.get('/users/$userId');
            return UserModel.fromJson(response.data);
          }
        }
        ```

3.  **`repositories/`**
    *   **কাজ কি?** রিপজিটরি হল `datasources` এবং `models` এর মধ্যে একটি ব্রিজ। এটি ডোমেইন লেয়ারকে ডাটা দেওয়ার জন্য দায়বদ্ধ। রিপজিটরি ডিসাইড করে যে ডাটা নেটওয়ার্ক থেকে নিবে নাকি লোকাল ক্যাশে থেকে নিবে (Cache Strategy)। এটি `datasources` থেকে পাওয়া Model কে Domain Layer-এর জন্য উপযোগী Entity-তে রূপান্তর করে। এইখানে `UserRepositoryImpl` ক্লাসটি `UserRemoteDataSource` এবং `UserLocalDataSource` (যদি থাকে) কে ব্যবহার করে। UI/Domain লেয়ার শুধুমাত্র রিপজিটরির মাধ্যমে ডাটা রিকোয়েস্ট করবে, সরাসরি DataSource-কে কল করবে না।

        ```dart
        // data/repositories/user_repository_impl.dart
        class UserRepositoryImpl implements UserRepository {
          final UserRemoteDataSource remoteDataSource;
          final UserLocalDataSource localDataSource;

          UserRepositoryImpl({
            required this.remoteDataSource,
            required this.localDataSource,
          });

          @override
          Future<User> getUser(int userId) async {
            // প্রথমে চেক করি লোকালে ডাটা আছে কিনা
            try {
              final localUser = await localDataSource.getUser(userId);
              return localUser;
            } catch (_) {
              // লোকালে না থাকলে রিমোট থেকে নিবো
              final remoteUser = await remoteDataSource.getUser(userId);
              // প্রয়োজন হলে রিমোট ডাটা লোকালে সেভ করবো
              await localDataSource.cacheUser(remoteUser);
              return remoteUser;
            }
          }
        }
        ```

এখন পর্যন্ত আমরা শিখলাম কিভাবে `core` ফোল্ডার আমাদের অ্যাপের কমন এবং ইউটিলিটি জিনিসপত্র ধরে রাখে। আর `data` ফোল্ডার কিভাবে Model, DataSource এবং Repository এর মাধ্যমে বাইরের দুনিয়া (API, Database) থেকে ডাটা নিয়ে এসে প্রস্তুত করে। **এখন আমরা** `domain` ফোল্ডারের অজানা রহস্য, Entity, Use Case এবং Dependency Injection নিয়ে বিস্তারিত আলোচনা করব।





#### **`domain` ফোল্ডারঃ Business Logic-layer**

`domain` লেয়ার সম্পূর্ণরূপে **UI এবং Data Source থেকে মুক্ত**। এর মানে এই লেয়ারে Flutter, Dio, Hive—কোনো প্যাকেজেরই code থাকবে না। এটি শুধুমাত্র আপনার অ্যাপের Business Rules নিয়ে কাজ করে। আমাদের রেস্টুরেন্টের উদাহরণে, এটি হল সেই "রেসিপি বুক" যেখানে লেখা আছে কোন ডিশটি কীভাবে তৈরি করতে হবে, কিন্তু সেখানে বাসন-কোসন বা রান্নাঘরের কোনো উল্লেখ নেই।

**`domain` ফোল্ডারের ভিতরে সাধারণত তিনটি জিনিস থাকে:**

1.  **`entities/`**
    *   **কাজ কি?** Entity হল আপনার অ্যাপের Core Business Object। এটি `data` লেয়ারের `model` এর সাথে মিলে যায়, কিন্তু এটি সম্পূর্ণ আলাদা। Model তৈরি হয় API-এর JSON স্ট্রাকচার অনুসারে, কিন্তু Entity তৈরি হয় আপনার অ্যাপের Business Logic অনুসারে। Data Layer শুধুমামত্র Entity-কে ব্যবহার করে, Model কে না। আমাদের User API থেকে `UserModel`-এ `createdAt` ফিল্ড আসে যা একটি String। কিন্তু আমাদের Business Logic-এ আমরা শুধু `id`, `name`, এবং `email` নিয়েই কাজ করব। তাই আমাদের Entity হবে সরল।

        ```dart
        // domain/entities/user.dart
        class User {
          final int id;
          final String name;
          final String email;

          User({required this.id, required this.name, required this.email});
        }
        ```
        লক্ষ্য করুন, এখানে কোন `fromJson` বা `toJson` মেথড নেই! কারণ Domain Layer-এর Data Format সম্পর্কে কোনো ধারণা নেই।

2.  **`repositories/`**
    *   **কাজ কি?** এটি একটি **অ্যাবস্ট্রাক্ট ক্লাস বা ইন্টারফেস** যা শুধুমাত্র বলে দেয় কোন কোন Operations (মেথড) পাওয়া যাবে। এর কোন ইমপ্লিমেন্টেশন এখানে থাকে না। ইমপ্লিমেন্টেশন থাকে `data` লেয়ারের `repository_impl` ক্লাসে। এটি একটি চুক্তিপত্র (Contract) এর মতো। আমরা এইখানে চুক্তি করলাম যে, User নিয়ে কাজ করার জন্য `getUser` নামের একটি মেথড থাকবে।

        ```dart
        // domain/repositories/user_repository.dart
        abstract class UserRepository {
          Future<User> getUser(int userId); // Returns User Entity, not UserModel
        }
        ```

3.  **`usecases/`**
    *   **কাজ কি?** Use Case (বা Interactor) হল একটি নির্দিষ্ট Business Task বা Action। প্রতিটি Use Case শুধুমাত্র **একটি** কাজ করবে। এটি Repository কে ব্যবহার করে Data নেয় এবং Business Logic প্রয়োগ করে। Use Case হল Domain Layer-এর সবচেয়ে গুরুত্বপূর্ণ কম্পোনেন্ট। আমাদের অ্যাপে "Get User Profile" একটি Use Case। আবার "Update User Profile" আরেকটি আলাদা Use Case।

        ```dart
        // domain/usecases/get_user.dart
        class GetUser {
          final UserRepository repository;

          GetUser(this.repository); // Repository is injected

          // এটি শুধুমাত্র একটি কাজ করে: একটি User নিয়ে আসে।
          Future<Either<Failure, User>> call(int userId) async {
            return await repository.getUser(userId);
          }
        }
        ```
        `Either` হল `dartz` প্যাকেজের একটি টাইপ, যা Left (Failure) বা Right (Success) রিটার্ন করে। এটি Error Handling-এর একটি চমৎকার পদ্ধতি।

#### **একটি স্ক্রিনে একাধিক Use Case কিভাবে ব্যবহার করব?**

এটি একটি খুবই কমন প্রশ্ন। উত্তরটি সহজ। আপনার UI (বা Bloc/Cubit) সরাসরি একাধিক Use Case কে ব্যবহার করবে।

**রিয়েল-লাইফ উদাহরণ:** একটি User Profile স্ক্রিন কল্পনা করুন। সেখানে User-এর ডিটেইল দেখাবে (GetUser Use Case), আবার User-এর বন্ধুদের লিস্টও দেখাবে (GetUserFriends Use Case)।

```dart
// presentation/bloc/user_profile_bloc.dart
class UserProfileBloc extends Bloc<UserProfileEvent, UserProfileState> {
  final GetUser getUserUseCase;
  final GetUserFriends getUserFriendsUseCase;

  UserProfileBloc({
    required this.getUserUseCase,
    required this.getUserFriendsUseCase,
  }) : super(UserProfileLoading()) {
    on<LoadUserProfile>((event, emit) async {
      // একই ইভেন্টে দুইটি Use Case কে কল করা
      final userResult = await getUserUseCase(event.userId);
      final friendsResult = await getUserFriendsUseCase(event.userId);

      // এখন দুইটি রেজাল্ট নিয়ে স্টেট তৈরি করুন
      userResult.fold(
        (failure) => emit(UserProfileError(failure.message)),
        (user) {
          friendsResult.fold(
            (failure) => emit(UserProfileError(failure.message)),
            (friends) => emit(UserProfileLoaded(user: user, friends: friends)),
          );
        },
      );
    });
  }
}
```

#### **ডিপেনডেন্সি ইনজেকশন (DI) কি এবং কেন প্রয়োজন?**

*   **ডিপেনডেন্সি ইনজেকশন কি?** সহজ ভাষায়, একটি ক্লাসের প্রয়োজনীয় জিনিসপত্র (Dependencies) বাইরে থেকে সরবরাহ করাকেই DI বলে। ক্লাস নিজে তার Dependency তৈরি করে না।
    *   **Without DI (খারাপ উদাহরণ):**

        ```dart
        class UserRepositoryImpl {
          // Repo নিজেই DataSource তৈরি করছে - Tight Coupling
          final UserRemoteDataSource dataSource = UserRemoteDataSource();

          Future<User> getUser(int userId) async {
            return await dataSource.getUser(userId);
          }
        }
        ```
        
    *   **With DI (ভালো উদাহরণ):**

        ```dart
        class UserRepositoryImpl {
          // DataSource বাইরে থেকে দেওয়া হচ্ছে - Loose Coupling
          final UserRemoteDataSource dataSource;

          UserRepositoryImpl({required this.dataSource});

          Future<User> getUser(int userId) async {
            return await dataSource.getUser(userId);
          }
        }
        ```

*   **কেন DI প্রয়োজন?**
    1.  **টেস্টেবিলিটি:** Unit Test লিখতে গেলে আপনি `UserRemoteDataSource` এর একটি Mock Version (নকল) বানিয়ে `UserRepositoryImpl`-কে ইনজেক্ট করে দিতে পারবেন। আসল API কে কল ছাড়াই টেস্ট করা যাবে।
    2.  **লুজ কাপলিং:** ক্লাসগুলো একে অপরের উপর কম নির্ভরশীল হয়। `UserRemoteDataSource` বদলে `FakeUserDataSource` দিলেও `UserRepositoryImpl` ঠিকভাবে কাজ করবে।
    3.  **কোড রিইউজ ও মেইনটেনেন্স:** কোড পরিবর্তন ও আপডেট করা খুব সহজ হয়ে যায়।

আমরা সাধারণত `get_it` প্যাকেজ ব্যবহার করে DI কে ম্যানেজ করি। একটি ফাইলে (যেমন `injector.dart` বা `service_locator.dart`) আমরা সব ডিপেনডেন্সি রেজিস্টার করে রাখি।

```dart
// core/injection_container.dart
final getIt = GetIt.instance;

void init() {
  // DataSources
  getIt.registerLazySingleton<UserRemoteDataSource>(() => UserRemoteDataSourceImpl(dio: getIt()));

  // Repository
  getIt.registerLazySingleton<UserRepository>(() => UserRepositoryImpl(remoteDataSource: getIt()));

  // Use Cases
  getIt.registerLazySingleton(() => GetUser(getIt()));
  getIt.registerLazySingleton(() => GetUserFriends(getIt()));

  // Blocs
  getIt.registerFactory(() => UserProfileBloc(getUserUseCase: getIt(), getUserFriendsUseCase: getIt()));
}
```

**এখন পর্যন্ত**  আমরা শিখলাম কিভাবে `domain` ফোল্ডার আমাদের Business Logic কে UI এবং Data থেকে আলাদা রাখে। Entity, Repository Contract এবং Single Responsibility সমৃদ্ধ Use Case এর শক্তি আমরা দেখলাম। একই সাথে DI এর মাধ্যমে কীভাবে আমরা আমাদের কোডকে টেস্টযোগ্য এবং ফ্লেক্সিবল করে তুলি সেটাও বুঝলাম। **এখন** আমরা `presentation` ফোল্ডার নিয়ে কথা বলব এবং ক্লিন আর্কিটেকচারে State Management-এর ভূমিকা নিয়ে একটি গুরুত্বপূর্ণ আলোচনা করব।






#### **`presentation` ফোল্ডারঃ**

এই লেয়ারটি সম্পূর্ণরূপে Flutter-এর উপর নির্ভরশীল। এর কাজ হল UI কে রেন্ডার করা এবং ইউজারের ইনপুট (যেমন- বাটন প্রেস) কে Use Case-এ পৌঁছে দেওয়া।

**`presentation` ফোল্ডার সাধারণত Feature-wise/Visa-wise ভাগ করা থাকে।**

```
presentation/
├── user_profile/          # একটি ফিচার (User Profile)
│   ├── bloc/             # এই ফিচারের State Management (Bloc)
│   ├── pages/            # ফুল স্ক্রিন (user_profile_page.dart)
│   ├── widgets/          # শুধুমাত্র এই ফিচারে ব্যবহৃত উইজেট
│   └── user_profile_screen.dart
├── login/                # আরেকটি ফিচার (Login)
│   ├── cubit/
│   ├── pages/
│   └── widgets/
```

*   **`bloc/` বা `cubit/`:** এই ফিচারের Business Logic (যা UI লজিক) এবং State কে ম্যানেজ করে।
*   **`pages/`:** সাধারণত Route-এর মাধ্যমে নেভিগেট হওয়া ফুল স্ক্রিনগুলো থাকে এখানে।
*   **`widgets/`:** শুধুমাত্র এই ফিচারে ব্যবহৃত, Reusable উইজেটগুলো থাকে এখানে (যেমন- `ProfileCard`, `LoginForm` ইত্যাদি)।



```dart
// presentation/pages/user_profile_page.dart
class UserProfilePage extends StatelessWidget {
  final int userId;

  const UserProfilePage({super.key, required this.userId});

  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => getIt<UserProfileBloc>()..add(LoadUserProfile(userId)),
      child: Scaffold(
        appBar: AppBar(title: const Text('Profile')),
        body: BlocBuilder<UserProfileBloc, UserProfileState>(
          builder: (context, state) {
            if (state is UserProfileLoading) {
              return const Center(child: CircularProgressIndicator());
            } else if (state is UserProfileLoaded) {
              final user = state.user;
              final friends = state.friends;
              return Column(
                children: [
                  Text('Name: ${user.name}'),
                  Text('Email: ${user.email}'),
                  const Text('Friends:'),
                  // ... friends list
                ],
              );
            } else if (state is UserProfileError) {
              return Center(child: Text('Error: ${state.message}'));
            }
            return const SizedBox();
          },
        ),
      ),
    );
  }
}
```


**ক্লিন আর্কিটেকচার আপনাকে বলে না আপনাকে Bloc, Provider, Riverpod, নাকি GetX ব্যবহার করতে হবে।** এটি একটি আর্কিটেকচারাল প্যাটার্ন, State Management লাইব্রেরি নয়।

State Management হল শুধুমাত্র **`presentation` লেয়ারের একটি টুল**। এর কাজ হল UI এর State (লোডিং, ডাটা লোডেড, এরর) কে ম্যানেজ করা এবং UI কে পুনরায় রেন্ডার করতে বলা।

**ক্লিন আর্কিটেকচার বলছেঃ** "তোমার UI লজিক (যা State Management করে) এবং Business Logic (যা Use Case করে) আলাদা রাখো।" তুমি সেই UI লজিকটি পরিচালনা করার জন্য যে কোনো টুলই ব্যবহার করতে পারো।

#### **একাধিক State Management দিয়ে বোঝানো যাক**

ধরুন আমাদের একটি কাউন্টার অ্যাপ। Business Logic খুবই সহজ: "Increment the count."

1.  **Provider দিয়েঃ**
    *   আমরা একটি `CounterProvider` ক্লাস বানাবো যা `ChangeNotifier` এক্সটেন্ড করবে।
    *   `CounterProvider`-এ `increment()` মেথড থাকবে এবং `notifyListeners()` কল করবে।
    *   UI (`CounterPage`) `Consumer` Widget দিয়ে `CounterProvider` কে listen করবে।

2.  **Bloc দিয়েঃ**
    *   আমরা একটি `CounterBloc` ক্লাস বানাবো।
    *   `CounterEvent` হিসেবে `Increment` ইভেন্ট থাকবে।
    *   `CounterState` হিসেবে শুধু `counter` value থাকবে।
    *   UI `BlocBuilder` দিয়ে State কে listen করবে এবং বাটন প্রেসে `Increment` ইভেন্ট ডispatch করবে।

3.  **Riverpod দিয়েঃ**
    *   আমরা একটি `counterProvider` (StateNotifierProvider) বানাবো।
    *   UI `Consumer` Widget দিয়ে `counterProvider` কে watch করবে।

**গুরুত্বপূর্ণ বিষয়** উপরের সব ক্ষেত্রেই, **`domain` এবং `data` লেয়ারের কোড একই থাকবে!** `GetIncrementedCount` Use Case টি সব Scenario-তেই ব্যবহার করা যাবে। শুধু `presentation` লেয়ারে এটি কে কল করছে সেটা `Provider` নাকি `Bloc` সেটা আলাদা।

```dart
// Domain Layer (সব Scenario-তেই একই)
class GetIncrementedCount {
  final CounterRepository repository;
  GetIncrementedCount(this.repository);

  int call(int currentCount) {
    return repository.getIncrementedCount(currentCount);
  }
}

// Presentation with Bloc (উদাহরণ)
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  final GetIncrementedCount getIncrementedCountUseCase;
  CounterBloc({required this.getIncrementedCountUseCase}) : super(CounterState(0)) {
    on<Increment>((event, emit) {
      // Use Case কে ব্যবহার করা হচ্ছে
      final newCount = getIncrementedCountUseCase(state.count);
      emit(CounterState(newCount));
    });
  }
}
```

**কেন সম্পর্ক নেই?**
কারণ ক্লিন আর্কিটেকচার হল অ্যাপের গঠনপ্রণালী (Structure), আর State Management হল UI কে রেস্পন্সিভ ও ইন্টারেক্টিভ করার একটি পদ্ধতি (Mechanism)। তুমি একটি দালানের কাঠামো (ক্লিন আর্কিটেকচার) একই রেখে ভিতরের দরজা-জানালা, ওয়াল পেইন্ট (State Management) বদলাতে পারো।

### **ধারাবাহিকতার শেষ কথা**

ফ্লাটারে ক্লিন আর্কিটেকচার প্রথমে জটিল মনে হলেও, একবার রপ্ত করে ফেললে আপনি বুঝতে পারবেন এটি কতটা শক্তিশালী এবং প্রয়োজনীয়। এটি আপনাকে অগোছালো, রক্ষণাবেক্ষণে কঠিন ("স্প্যাগেটি") কোড থেকে রক্ষা করে।

**সংক্ষেপে পুরো ধারাবাহিকতা:**

1.  **`core`:** অ্যাপের ইউটিলিটি এবং কমন জিনিসপত্র।
2.  **`data`:** বাইরের বিশ্ব (API, DB) থেকে ডাটা নিয়ে আসে, Model, DataSource, Repository-তে প্রক্রিয়া করে।
3.  **`domain`:** শুদ্ধ Business Logic, Entity, Use Case, এবং Repository Contract নিয়ে গঠিত। (সবচেয়ে গুরুত্বপূর্ণ লেয়ার)
4.  **`presentation`:** UI এবং State Management নিয়ে কাজ করে। Use Case গুলোকে ব্যবহার করে ইউজারের সাথে ইন্টারেকশন করে।

