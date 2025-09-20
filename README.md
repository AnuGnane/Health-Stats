6.1 Initial Development

When first starting the project, it was already decided that iOS was the development platform for several reasons as mentioned in the literature review. Having never worked with IOS before, one of the first steps of implementation was starting a 40- hour tutorial developed by Apple to understand the basics of Xcode and Swift. (Xcode is Apple's integrated development environment used to develop software for iOS. Swift is a general-purpose programming language used in Xcode to develop iOS apps.)

This helped to understand the fundamentals and best practice for the platform, allowing for a smooth development process. While going through all this it was clear that the most difficult part would be pulling the data from the HealthKit API, this is due to many privacy layers built in iOS making it harder to test this function, whereas things such as UI elements and basic page design could be easily previewed in an emulator built into Xcode.

6.2 Databases 

A cloud firestore database was created to store data for a variety of metrics, these are maintained in collections called ‘users’, ‘leaderboard’ and ‘foods’. Figure 6.0 shows a layout of this NoSQL database. 


<img width="248" height="136" alt="image" src="https://github.com/user-attachments/assets/b8f3df98-2cf6-45fc-b644-77ba03a53546" />


Core Data was used for offline data storage so that the user can access certain features of the app without having to be constantly connected to the Internet, this was done for the calorie tracking page, where the data for logging food items was stored on device. Figure 6.0.1 shows the calorie data model used to implement this feature.  

<img width="307" height="221" alt="image" src="https://github.com/user-attachments/assets/8622e4a9-a2eb-4bb8-b01e-feddb531dcc7" />


6.3 Finished App 

This section covers the development process, important sections of code and screenshots of the final app.

6.3.1 Navigation

One of the very first steps of development was to create the main view for the app, as well as creating navigation links between them all.  Usually the ‘NavigationView” is used in SwiftUI to handle movements between different views, which has been used for the other pages, but a much simpler method was employed here. The ‘TabView” was used as it makes navigation more efficient and less code heavy, enabling the user to simply cycle through the pages. This was a bottom navigation bar, Figure 6.1 shows how this looks on the app, with the names of each of the views and a related icon signifying each tab.

<img width="406" height="65" alt="image" src="https://github.com/user-attachments/assets/25a1aa4d-ad8d-41bb-96ad-a802962e56d5" />

6.3.2 Login Page

The Login Page (Figure 6.2) and Sign-Up Page (Figure 6.3) were designed to provide users with an easy and secure way to access the app. These pages are connected to Firebase Authentication, offering built-in validation for email addresses and passwords. This ensures that email addresses are formatted correctly, and passwords contain a minimum of six characters, thereby preventing the storage of incorrect information.
A 'Create an account' link, located at the bottom of the Login Page, navigates users to the Sign-Up Page (Figure 5.3) for creating a new account.

<img width="157" height="341" alt="image" src="https://github.com/user-attachments/assets/8ac38246-d46e-4b99-923f-5c42b3c7282d" />

<img width="157" height="341" alt="image" src="https://github.com/user-attachments/assets/61fa9837-f344-4b4d-83e7-f10ca16e6b81" />

6.3.4 Calories Page

The Calories page serves as the central hub for tracking the user's calorie and macronutrient intake. It provides essential metrics such as total calories consumed, calories burned, and the user's calorie balance for the current day. 

HealthKit API is utilized to access the user's calories burned from both active and basal energy. This is achieved through the getEnergyBurned() function as shown on Figure 6.4.6. Two HealthKit queries are executed to fetch active and basal energy burned, and the results are then combined to calculate the total calories burned for the day so far. The updateTotalEnergyBurned() function ensures that the total calories burned are updated whenever the page appears. 

The user's calorie balance is determined in the calculateCalorieStatus() function (Figure 6.4.7). By subtracting total calories consumed from total calories burned, the calorie difference helps ascertain if the user is in a surplus, deficit, or maintaining their calories. The appropriate message is then returned and displayed on the page, As highlighted on Figures 6.4 and 6.4.1 there is also a progress bar comparing calories consumed to total calories burned, this is blue by default but becomes red if the user is in a surplus. 

A search bar at the top of the page lets users search the McCance and Widdowson’s food dataset, which was imported into the Firebase Firestore. This is achieved through the seachFoods() function (Figure 6.4.8).  After searching for and selecting a food item, a popup window (Figure 6.4.3) will appear letting the user adjust the serving size. The Firebase integration allows for a vast food database without the need to bundle it within the app’s local files. If a food item cannot be found from the search, there is also a plus icon located at the top of the page that will take the user to a new view where they can add in a custom food item (Figure 6.4.4). The user can give it a name and add in all its nutrient information. 

<img width="145" height="315" alt="image" src="https://github.com/user-attachments/assets/e8c7f615-71c9-4e1b-90d0-bcf0c4813cdb" />
<img width="145" height="316" alt="image" src="https://github.com/user-attachments/assets/2df9459e-757d-4cfd-acea-81375e925040" />
<img width="146" height="316" alt="image" src="https://github.com/user-attachments/assets/25b319a9-7f66-444f-a2d7-0555e4935ff2" />

<img width="143" height="311" alt="image" src="https://github.com/user-attachments/assets/617fd419-df4a-4d72-b542-d6d8461b9f53" />
<img width="144" height="311" alt="image" src="https://github.com/user-attachments/assets/f9cd4734-578d-493a-8ac4-730a724d633f" />
<img width="144" height="312" alt="image" src="https://github.com/user-attachments/assets/6730d0a7-4661-44a1-bbc9-24003ccf1ccd" />

6.3.5 Meal Plans Page

The Meal Plan view, (Figure 6.5), presents users with a list of various meal plans, which are fetched using the Spoonacular API. Each meal plan is displayed in a List view, accompanied by an image and the name of the meal. This is achieved by the loadData() function which is called to fetch meal plans from the Spoonacular API (Figure 6.5.2). This data is then stored in an array of SpoonacularMeal objects. 

Users can tap on a specific meal plan, which expands into a more detailed view that includes a full-sized image of the meal, its title, step-by-step instructions, and a complete ingredients list (Figure 6.5.1). As shown in Figure 6.5.3 an API struct was setup to easily enable the fetching of meal plans and their details, the fetchRecipes() and fetchRecipeDetails() functions are used for these purposes, respectively. The instructions and ingredients are then displayed in a ScrollView, allowing users to easily navigate the content.

<img width="138" height="299" alt="image" src="https://github.com/user-attachments/assets/d947329f-358c-4c2b-8e36-4f90aab18993" />

<img width="138" height="299" alt="image" src="https://github.com/user-attachments/assets/a7ad26b4-055b-489e-ae45-b05c584ceee9" />

6.3.6 Health Stats Page

The Health Stats page (Figure 6.6) displays an overview of the user's health metrics, which are fetched using the HealthKit API. The available metrics include active calories burned, resting calories, exercise time, stand time, distance moved, and step count. It uses a ScrollView containing a LazyVGrid to display a grid layout of the different health metrics 
(Figure 6.6.3). Each grid item is created using a ForEach loop that iterates over all available activities. NavigationLinks are then used to navigate users to the DetailView (Figures 6.6.1 and 6.6.2), when a specific metric is tapped. This shows the user a bar graph and list of their activity stats from the last week.

The HKRepository class (Figure 6.6.5) handles the interaction with the HealthKit API. It requests authorization for the required data types, fetches the health stats based on the activity category, and processes the health stats. The DetailViewModel (Figure 6.6.4) is responsible for requesting the health stats from the HealthKit API using the HKRepository class. It formats the fetched data and stores it in an array of HealthStat objects. 

The DetailViewModel utilizes DateFormatter and MeasurementFormatter to present dates and health metric values in a user-friendly manner. DateFormatter is employed to display dates in a format that is easily understandable and consistent across various regions, while MeasurementFormatter ensures that health metric values are presented in the appropriate units for each user, taking into account their locale and preferences. This approach guarantees that users from different regions can access their health data in a familiar and consistent format, making the app more accessible and easier to interpret.

Separating the logic for fetching and formatting health data into a dedicated view model (DetailViewModel) and a repository (HKRepository) allows for a cleaner, more modular architecture. This separation of concerns makes it easier to maintain the code and scale the app, as each class has a specific responsibility. The view model handles the presentation logic, while the repository is responsible for interacting with the HealthKit API.


<img width="158" height="342" alt="image" src="https://github.com/user-attachments/assets/bc24f609-2079-487f-ad51-ecf801bd458a" />
<img width="158" height="342" alt="image" src="https://github.com/user-attachments/assets/887ea275-b9b2-4860-a847-9d263d75f677" />
<img width="158" height="342" alt="image" src="https://github.com/user-attachments/assets/d4bdaf4b-d417-47cf-8430-027b0fa6e7ba" />

6.3.7 Leaderboard Page: 

The Leaderboard view (Figures 6.7-6.7.2) showcases three different leaderboards - Active Energy (calories burned), Steps, and Exercise Time. Users can access each leaderboard using a segmented control slider at the top of the screen. The data for the leaderboards is fetched from HealthKit and stored in Firebase Firestore, which updates the table accordingly.

A HealthMetric enum was defined to represent the three health metrics: Active Energy, Steps, and Exercise Time. This serves as a convenient way to represent the three health metrics and their corresponding strings. The enum simplifies the process of displaying and managing the leaderboard data for each metric by providing a consistent way to reference them.

The View's body (Figure 6.7.3) contains a NavigationView with a VStack, which displays the segmented control slider and the list of leaderboard entries. The segmented control slider allows users to switch between the three leaderboards (Active Energy, Steps, and Exercise Time) quickly and easily without the need for additional navigation. The use of this slider not only simplifies the user interface but also reduces the cognitive load on the users, allowing them to focus on their performance in each health metric.

<img width="153" height="332" alt="image" src="https://github.com/user-attachments/assets/9bb553e0-3d60-4bd7-b660-0755f53c9b28" />
<img width="153" height="332" alt="image" src="https://github.com/user-attachments/assets/f77b9895-bf4e-4562-aace-cafb38f926ce" />
<img width="153" height="332" alt="image" src="https://github.com/user-attachments/assets/76726525-5f54-43e3-894f-9331dbbc3a62" />

6.3.8 Account Page: 

The Account Details page, as shown in Figure 6.8, presents the user with their account information, which they provided when registering their account. The view is made using a VStack with the user's account information if the userData variable has been populated, otherwise, it shows a loading message. The account information includes the user's name, age, sex, weight, height, goal, and estimated BMR.

To fetch the user data from the Firestore database, the fetchUserData() function (Figure 6.8.2) is called when the view appears. The function retrieves the user document from the "users" collection in Firestore using the user's unique identifier (UID). Once the document is retrieved, the user's data is extracted and used to update the userData state property.

The page also features a button that navigates to the Edit Account page and a sign-out button to log out of the user's account. When the user clicks on the "Sign Out" button, the signOut function is called (Figure 6.8.3). This function signs the user out from Firebase Auth and navigates back to the main page, this also toggles the 'userIsLoggedIn' variable to false keeping them logged out.

The Account Details page provides users with an overview of their account information and allows them to navigate to the Edit Account page (Figure 6.8.1) to make any necessary changes. The Edit Account page allows users to modify their account information, such as their name, age, sex, weight, height, and fitness goal.

<img width="191" height="414" alt="image" src="https://github.com/user-attachments/assets/c30aec50-578e-4e11-9637-4e57a045bb8d" />
<img width="191" height="414" alt="image" src="https://github.com/user-attachments/assets/95958fed-05cd-44e3-a64f-b66fd554ae77" />


6.3.9 

Adjustments made for Accessibility:

As discussed in the literature review the app has been designed to dynamically adjust to light and dark modes, this is to help users with visual impairments and improve overall user experience. This feature allows the app to switch between white and black backgrounds based on the user's device settings or preferences.

SwiftUI provides a simple way to support dynamic light and dark modes. By utilizing the Color struct, we can leverage system colours, which automatically adapt to the current environment settings. The above figures have shown the app in dark mode, below is how the app looks while in light mode.
Login <img width="115" height="246" alt="image" src="https://github.com/user-attachments/assets/70182559-2732-4273-b282-e25bb984b015" />

Calories <img width="115" height="246" alt="image" src="https://github.com/user-attachments/assets/76acc7e6-0be2-4494-8287-27348fa735bf" />

Meal Plans <img width="115" height="247" alt="image" src="https://github.com/user-attachments/assets/b0bd8029-b194-4d11-986d-f09f5ddeb49f" />

Health Stats <img width="114" height="246" alt="image" src="https://github.com/user-attachments/assets/ea108729-61c4-4a93-9388-14a4ccc8cb25" />

Leaderboard <img width="115" height="246" alt="image" src="https://github.com/user-attachments/assets/ca760918-177d-48b6-9977-7e678da88e71" />

Account details <img width="115" height="246" alt="image" src="https://github.com/user-attachments/assets/2f3699db-429f-4994-9613-07eb1376b54e" />





























