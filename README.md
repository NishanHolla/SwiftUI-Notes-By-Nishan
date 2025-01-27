# SwiftUI-Notes-By-Nishan

We will be using the example of a parcel management system for this.

Folder Structure

ParcelManagementApp/
│
├── Models/                      # Data models for the app.
│   ├── Parcel.swift             # Parcel data model.
│   ├── User.swift               # User data model.
│   └── APIResponse.swift        # Generic API response model.
│
├── Views/                       # All SwiftUI views (UI components).
│   ├── HomeView.swift           # Main screen of the app.
│   ├── ParcelDetailView.swift   # View for parcel details.
│   ├── AddParcelView.swift      # View for adding a new parcel.
│   ├── Components/              # Reusable components.
│   │   ├── ParcelRow.swift      # Row component for displaying parcel info.
│   │   ├── LoadingView.swift    # Reusable loading indicator.
│   │   └── ErrorView.swift      # Error message display.
│   └── Modals/                  # Reusable modals/popups.
│       └── ConfirmDeleteModal.swift # Modal for delete confirmation.
│
├── ViewModels/                  # Manages state and business logic.
│   ├── ParcelListViewModel.swift # ViewModel for managing parcel list.
│   ├── ParcelDetailViewModel.swift # ViewModel for parcel detail screen.
│   ├── UserViewModel.swift      # ViewModel for managing user state.
│   └── AddParcelViewModel.swift # ViewModel for adding a new parcel.
│
├── Services/                    # Handles API/networking and data persistence.
│   ├── APIClient.swift          # Networking layer for API calls.
│   ├── ParcelService.swift      # Service for parcel-related API operations.
│   └── UserService.swift        # Service for user-related API operations.
│
├── Resources/                   # Assets like images, colors, and localized strings.
│   ├── Images.xcassets          # Image assets (e.g., icons, logos).
│   ├── Colors.swift             # Centralized app colors.
│   ├── Localization/            # Localized strings for different languages.
│   │   └── en.lproj/Localizable.strings
│   └── Fonts/                   # Custom fonts.
│
├── App/                         # Entry point and app-wide configurations.
│   ├── ParcelManagementApp.swift # App entry point (`@main`).
│   └── AppEnvironment.swift     # Global environment configurations.
│
└── Utils/                       # Helper functions and extensions.
    ├── DateExtensions.swift     # Extensions for working with dates.
    ├── JSONDecoder+Extensions.swift # Extensions for decoding JSON.
    ├── Logger.swift             # Logging utility.
    └── Constants.swift          # App-wide constants (e.g., API URLs).

Key Files and Their Responsibilities

Models
	•	Define the structure of your data and conform to Codable for decoding API responses.
	•	Example: Parcel.swift

struct Parcel: Identifiable, Codable {
    let id: String
    let sender: String
    let recipient: String
    let status: String
    let deliveryDate: Date
}



Views
	•	Composable SwiftUI views for the user interface.
	•	Organized by screens (HomeView, ParcelDetailView) and reusable components (ParcelRow, LoadingView).

ViewModels
	•	ObservableObject classes that manage state and handle business logic for each view.
	•	Expose @Published properties that views subscribe to for updates.

Example: ParcelListViewModel.swift

class ParcelListViewModel: ObservableObject {
    @Published var parcels: [Parcel] = []
    @Published var isLoading = false
    @Published var errorMessage: String?

    func fetchParcels() async {
        isLoading = true
        do {
            let fetchedParcels = try await ParcelService.getParcels()
            DispatchQueue.main.async {
                self.parcels = fetchedParcels
                self.isLoading = false
            }
        } catch {
            DispatchQueue.main.async {
                self.errorMessage = error.localizedDescription
                self.isLoading = false
            }
        }
    }
}

Services
	•	Perform API calls and handle networking logic.
	•	Separate business logic from networking details.

Example: ParcelService.swift

class ParcelService {
    static func getParcels() async throws -> [Parcel] {
        let url = URL(string: "https://api.example.com/parcels")!
        let (data, _) = try await URLSession.shared.data(from: url)
        return try JSONDecoder().decode([Parcel].self, from: data)
    }
}

Resources
	•	Centralized assets for images, colors, localization, and fonts.
	•	Helps maintain consistency across the app.

App
	•	Entry point of the app (ParcelManagementApp.swift) and global configurations (e.g., environment setup).

Example: ParcelManagementApp.swift

@main
struct ParcelManagementApp: App {
    @StateObject private var userViewModel = UserViewModel()

    var body: some Scene {
        WindowGroup {
            HomeView()
                .environmentObject(userViewModel)
        }
    }
}

Utils
	•	Contains helper functions, extensions, or constants used throughout the app.
	•	Keeps code clean and DRY.

Example Flow
	1.	HomeView displays a list of parcels using ParcelListViewModel.
	2.	Tapping a parcel navigates to ParcelDetailView, which uses ParcelDetailViewModel.
	3.	ParcelService fetches data from the API and decodes it into the Parcel model.
