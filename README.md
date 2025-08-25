E-Pharmacies: On-Duty Pharmacy Finder

![alt text](https://img.shields.io/badge/Kotlin-1.9.0-7F52FF.svg)

![alt text](https://img.shields.io/badge/Jetpack_Compose-1.6-4285F4.svg)

![alt text](https://img.shields.io/badge/Firebase-9.0.0-FFCA28.svg)

E-Pharmacies is a modern, native Android application designed to help users in Greece effortlessly locate nearby on-duty pharmacies. Built with Kotlin and Jetpack Compose, the app provides a real-time, interactive map experience powered by OpenStreetMap and Firebase, ensuring users can find an open pharmacy quickly and reliably, especially during emergencies.
📸 Screenshots

A glimpse into the user experience, from onboarding to finding a pharmacy.
Welcome & Onboarding	Interactive Map	Pharmacy Details	Navigation Drawer & Features
A clean introduction to the app's powerful features.	A fluid, real-time map displaying on-duty pharmacies.	Detailed information at a glance with quick actions.	Easy access to user profile, settings, and more.
<img src="https://i.imgur.com/Gj3C8kU.png" width="250">	<img src="https://i.imgur.com/tH3G03v.png" width="250">	<img src="https://i.imgur.com/qEw6h5W.png" width="250">	<img src="https://i.imgur.com/86HwP5t.png" width="250">
Login & Authentication	On-Duty Filter	Radius Search	Dark & Light Mode
Secure sign-in and registration powered by Firebase.	Instantly filter to see only pharmacies that are currently open.	Find the closest pharmacy or expand your search up to 5km.	A seamless experience in any lighting condition.
<img src="https://i.imgur.com/MExhP8d.png" width="250">	<img src="https://i.imgur.com/gY96t2e.png" width="250">	<img src="https://i.imgur.com/gY96t2e.png" width="250">	<img src="https://i.imgur.com/86HwP5t.png" width="250">
✨ Key Features

    Interactive Real-time Map: Displays all nearby pharmacies on an interactive OpenStreetMap view, with the user's location at the center.

    On-Duty Filtering: A one-tap filter to instantly hide closed pharmacies, showing only those currently on-duty and available.

    Radius Search: Easily find the single closest pharmacy or expand the search radius (e.g., 100m, 500m, 1km, 5km) to suit your needs.

    Secure User Authentication: Full sign-up and login functionality using Firebase Authentication, including email verification and password reset.

    Personalized Experience: Logged-in users can save their favorite pharmacies for quick access.

    Detailed Pharmacy Information: Tapping a pharmacy reveals a bottom sheet with its name, address, phone number, and working hours.

    Quick Actions: Navigate directly using Google Maps or call the pharmacy with a single tap from the details sheet.

    Multi-language Support: Fully localized for both English and Greek users, with an in-app language switcher.

    Dark/Light Mode: The app respects system settings and offers a manual toggle for a comfortable viewing experience day or night.

    Monetization Ready: Includes integrated AdMob banners and an in-app purchase option (via Google Play Billing) to remove ads, demonstrating premium feature handling.

🏗️ Architecture & Technical Deep Dive

This application is built following modern Android development best practices, emphasizing a clean, scalable, and maintainable codebase.
MVVM (Model-View-ViewModel) Architecture

The app is structured using the MVVM architecture pattern, which provides a clear separation of concerns:

    View (UI Layer): Built entirely with Jetpack Compose. This declarative UI framework allows for building the UI as a set of stateless composable functions that react to state changes. This drastically reduces boilerplate code compared to XML layouts and makes the UI more predictable and easier to manage. The MapScreen, AuthScreen, etc., are responsible only for displaying the state provided by the ViewModel.

    ViewModel: The ViewModels (MapViewModel, AuthViewModel) act as the bridge between the UI and the Data Layer. They hold the UI state in StateFlow, handle user interactions, and execute business logic. By using viewModelScope, all asynchronous operations (like network calls to Firebase) are automatically managed and scoped to the ViewModel's lifecycle, preventing memory leaks.

    Model (Data Layer): This layer is responsible for data sourcing. It interacts directly with Firebase (Firestore and Authentication) for remote data and Jetpack DataStore for local user preferences (like saved email or onboarding status). This isolates data operations from the rest of the app.

+------------------+      +-------------------+      +-------------------------+
|      View        |      |     ViewModel     |      |      Data Layer         |
| (Jetpack Compose)|      | (Android ViewModel|      | (Firestore, Auth,      |
|  - MapScreen     |<---->|  StateFlow)       |<---->|  DataStore)             |
|  - AuthScreen    |      |  - MapViewModel   |      |                         |
|  - ...           |      |  - AuthViewModel  |      |  - Firebase Repository  |
+------------------+      +-------------------+      |  - User Preferences     |
                                                     +-------------------------+


Key Technical Decisions & Why

    Kotlin as the Sole Language: Chosen for its conciseness, null safety, and excellent support for functional programming and coroutines, which are essential for modern Android development.

    Jetpack Compose for the UI: Instead of traditional XML, Compose was chosen to build a more dynamic, interactive, and maintainable UI with significantly less code. Its state-driven nature works perfectly with the reactive MVVM pattern.

    Kotlin Coroutines & StateFlow: Used for managing all asynchronous operations. StateFlow provides a reactive stream of data from the ViewModel to the UI. When pharmacy data, filter status, or user location changes in the ViewModel, the UI automatically recomposes to reflect the new state without any manual view manipulation.

    Firebase for Backend: A powerful and scalable choice.

        Firebase Authentication: Provides a complete, secure, and easy-to-implement solution for user management, saving significant development time.

        Firestore: A NoSQL cloud database used to store user profiles and favorite pharmacies. Its real-time listeners (addSnapshotListener) mean that changes (like a new favorite being added) are pushed to the app instantly for a reactive experience.

    OpenStreetMap (OSM) via osmdroid: Chosen over Google Maps primarily because it is a powerful, free, and open-source alternative. This avoids potential billing complexities and API usage costs, making it ideal for indie projects.

    Jetpack DataStore: The modern replacement for SharedPreferences. It's used to persist simple key-value data asynchronously using Kotlin Coroutines, preventing UI thread blockage.

    Google Play Billing Library: Integrated to handle in-app purchases. This implementation showcases the ability to manage complex monetization flows, from querying products to acknowledging purchases and unlocking premium features for the user.

🛠️ Technology Stack & Key Libraries

    Language: Kotlin

    UI Framework: Jetpack Compose

    Architecture: MVVM (Model-View-ViewModel)

    Asynchronous Programming: Kotlin Coroutines & Flow

    Backend & Database:

        Firebase Authentication (for user management)

        Cloud Firestore (for user profiles & favorites)

    Mapping: OpenStreetMap (osmdroid)

    Navigation: Compose Navigation with animation support

    Local Storage: Jetpack DataStore (for user preferences)

    Monetization:

        Google AdMob (for banner & interstitial ads)

        Google Play Billing Library (for "Remove Ads" IAP)

    Location Services: Google Play Services Location

💡 Code Highlights & Noteworthy Implementations
1. Dynamic & Performant Map Markers

The map markers for pharmacies are not static images. Their size is dynamically adjusted based on the current map zoom level. This subtle detail significantly improves user experience: markers are smaller and less cluttered when zoomed out, and larger and easier to tap when zoomed in. An in-memory cache (iconCache) is used to avoid resizing the drawable on every recomposition, ensuring smooth map performance.
2. Fully Reactive Filtering System

The core filtering logic in MapViewModel is a great example of reactive programming. Whenever a user toggles the "On-Duty" filter, selects a new radius, or their location updates, the applyFilters() function is called. This function re-evaluates the list of pharmacies against the current state (held in a StateFlow). The UI, which is collecting this state, automatically and efficiently recomposes to display only the relevant markers, creating a seamless and instantaneous filtering experience.
3. Robust Localization Support

The app implements a LocaleManager that correctly handles language switching. It persists the user's language choice and uses the attachBaseContext method in MainActivity to apply the correct locale before the activity is created. This robust approach ensures all resources, from simple text strings to date/number formatting, are displayed correctly across the entire application and prevents common issues associated with in-app language changes.
