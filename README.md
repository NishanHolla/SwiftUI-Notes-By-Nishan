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
<ol>
	<li>	HomeView displays a list of parcels using ParcelListViewModel.
	<li>	Tapping a parcel navigates to ParcelDetailView, which uses ParcelDetailViewModel.
	<li>	ParcelService fetches data from the API and decodes it into the Parcel model.
</ol>
