# SwiftUI-Notes-By-Nishan

### This notes will help you if you come from a background of React or Next.js and want to build apps in SwiftUI
### You will need to use Xcode and a Mac by default for this

## First we will go through difference in project structure and hooks.
## Project Structure in Next.js and its equivalent in SwiftUI
<img width="689" alt="Screenshot 2025-01-27 at 9 51 51 PM" src="https://github.com/user-attachments/assets/d8224523-c944-4270-a9cd-cb4731240564" />

## useState and useContext hooks in equivalents in SwiftUI.
<img width="836" alt="Screenshot 2025-01-27 at 10 00 02 PM" src="https://github.com/user-attachments/assets/05f2ba44-e070-4f90-b5ad-477f95df76e7" />


## Lets learn with the structure and synatx of SwiftUi by using an example of a parcel management system.

Folder Structure :

<img width="750" alt="Screenshot 2025-01-27 at 10 46 47 PM" src="https://github.com/user-attachments/assets/cad6be82-2aab-4ca6-8787-490a74f5ade8" />


Key Files and Their Responsibilities

This document compares the key concepts and structures between SwiftUI and Next.js in terms of application architecture, design patterns, and best practices. It provides newcomers with insights into the similarities and differences between both frameworks.

1. Models

SwiftUI	Next.js
Define data models using structs that conform to Codable for JSON decoding.	Define models using TypeScript interfaces or classes for type safety and structure.
Example: struct Parcel: Codable { ... }. Swift’s Codable protocol makes JSON parsing seamless.	Example: interface Parcel { id: string; sender: string; recipient: string; status: string; deliveryDate: Date }. JSON parsing uses libraries like Axios or Fetch.
JSON decoding can be done directly in services using JSONDecoder.	In Next.js, you manually map API responses (e.g., res.json() with Axios/Fetch) to the model or interface.

Point to Note for Newcomers:
	•	SwiftUI has strict type checking and requires all fields in the model to match the JSON structure.
	•	In Next.js, mismatches won’t throw immediate errors unless TypeScript is used properly with tools like Zod for runtime validation.

2. Views

SwiftUI	Next.js
Views are created as SwiftUI structs and are composable. Example: struct HomeView: View { ... }.	Views in Next.js are React functional components. Example: function Home() { return <div>...</div> }.
Navigation between views happens using NavigationStack or NavigationLink.	Navigation is handled via the Next.js router, using Link or programmatic navigation (useRouter).
Reusable components are SwiftUI views stored in a Components folder (e.g., ParcelRow.swift).	Reusable components are React components stored in a components folder (e.g., ParcelRow.tsx).

Point to Note for Newcomers:
	•	SwiftUI: Navigation uses a declarative approach (NavigationStack), while React relies on imperative router management (e.g., useRouter.push()).
	•	Passing data between views in SwiftUI may require ObservableObject or EnvironmentObject, while React uses props or context.

3. ViewModels

SwiftUI	Next.js
Business logic and state management live in ObservableObject classes.	Business logic and state management are done with hooks (e.g., useState, useReducer, useContext).
Use @Published for state that updates the view automatically.	Use useState or state libraries like Zustand/Redux for similar functionality.
Example: class ParcelListViewModel: ObservableObject { @Published var parcels: [Parcel] }.	Example: const [parcels, setParcels] = useState([]);.

Point to Note for Newcomers:
	•	SwiftUI uses @Published for binding, while React relies on manually updating state.
	•	ObservableObjects require a @StateObject (for ownership) or @ObservedObject (for subscription).

4. Services

SwiftUI	Next.js
Networking is done using URLSession. You must construct a URL (URL(string: "...")) and await its response.	Networking uses fetch, axios, or any HTTP client. A URL can be passed directly as a string (no constructor).
Decoding JSON into models is done with JSONDecoder().decode().	Decoding JSON is straightforward with res.json() in fetch or similar methods in axios.
Example: let (data, _) = try await URLSession.shared.data(from: url).	Example: const res = await fetch(url); const data = await res.json();.

Point to Note for Newcomers:
	•	In SwiftUI, every URL must be constructed with URL(string: "..."), which can throw errors if the string is invalid.
	•	In Next.js, direct strings are fine, but you must handle async responses (res.json()).

5. Resources

SwiftUI	Next.js
Assets like images, colors, and fonts are stored in Resources (e.g., Images.xcassets).	Assets are stored in a public folder or imported directly as modules in React components.
SwiftUI uses built-in tools for managing colors and localization (e.g., Color.red, LocalizedStringKey).	Next.js uses libraries like next-i18next for localization and CSS-in-JS or Tailwind for colors/styles.

Point to Note for Newcomers:
	•	In SwiftUI, adding new images or colors requires updating .xcassets.
	•	In Next.js, assets can be dynamically imported or directly used from the /public folder.

6. App

SwiftUI	Next.js
The app’s entry point is defined in @main struct ParcelManagementApp: App.	The app’s entry point is defined in pages/_app.jsx or pages/_app.tsx.
Use Scene for defining the app window, which starts with the HomeView.	Use the App component to wrap all pages and define global providers (e.g., context, theming).

Point to Note for Newcomers:
	•	In SwiftUI, navigation and environment setup happen in @main.
	•	In Next.js, environment setup (e.g., global context providers) is done in _app.tsx.

7. Utils

SwiftUI	Next.js
Helper methods and extensions are stored in a Utils folder (e.g., DateExtensions.swift).	Helper functions are stored in a utils folder (e.g., dateUtils.js).
Swift uses extensions to add functionality to existing types (e.g., extension Date).	JavaScript/TypeScript uses utility functions or prototypes for similar functionality.

Point to Note for Newcomers:
	•	Swift’s extensions modify built-in types, while JavaScript relies on separate helper methods or prototypes.

8. Example Flow

SwiftUI	Next.js
HomeView: Uses ParcelListViewModel to fetch and display data.	Home.js: Uses useEffect or getStaticProps to fetch and display data.
Data is passed via ObservableObject to child views (ParcelDetailView).	Data is passed via props or context to child components.

Conclusion
	•	Both SwiftUI and Next.js follow a similar architectural pattern based on modern UI frameworks (declarative rendering, state management, and componentization), but the specifics of data flow, networking, and state management differ.
	•	SwiftUI emphasizes a strongly typed and declarative approach, while Next.js builds on JavaScript/React’s flexible, imperative style.
	•	Familiarity with data models, state management, and networking is essential in both frameworks, but newcomers should be mindful of SwiftUI’s strict type checking and observable object patterns, and how Next.js uses more flexible patterns with React hooks and JavaScript-based services.


Example Flow
<ol>
	<li>	HomeView displays a list of parcels using ParcelListViewModel.
	<li>	Tapping a parcel navigates to ParcelDetailView, which uses ParcelDetailViewModel.
	<li>	ParcelService fetches data from the API and decodes it into the Parcel model.
</ol>
