https://github.com/V-m1r/KinesteXSDK/assets/62508191/a796a98c-55c4-42d5-8ecd-731d2997e488

## Configuration

#### Info.plist

Add the following keys for camera usage:

```xml
<key>NSCameraUsageDescription</key>
<string>Camera access is required for video streaming.</string>
<key>NSMicrophoneUsageDescription</key>
<string>Microphone access is required for video streaming.</string>
```

### Add the framework as a package dependency:

```xml
https://github.com/V-m1r/KinesteXAIWebView.git
```
<img width="1305" alt="Screenshot 2024-02-24 at 10 28 52 AM" src="https://github.com/V-m1r/KinesteXAIWebView/assets/62508191/1cad2100-1beb-4386-8e55-ba3d8f37edc5">

### Available categories to sort workout plans: 

| **enum PlanCategory** | 
| --- | 
| **Strength** | 
| **Cardio** |
| **Rehabilitation** | 
| **Weight Management** | 
| **Custom(String) - in case we release new custom plans for your usage** | 


### Available categories to sort workouts (displayed right below the plans): 

| **enum WorkoutCategory** | 
| --- | 
| **Fitness** |
| **Rehabilitation** |
| **Custom(String) - in case we release new custom workouts for your usage** | 

## Usage

### Initial Setup

1. **Prerequisites**:
    - Ensure you've added the necessary permissions in `Info.plist`.
    - miniOS version - 13.0
      
2. **Launching the view**:
   - To display KinesteX, call `createWebView` in KinesteXAIFramework:

   ```Swift
    // isLoading is a State variable that can be used to display a loading screen before the webview loads
    KinesteXAIFramework.createWebView(apiKey: "your key", companyName: "your company", userId: "your userId", planCategory: .Cardio, workoutCategory: .Fitness, isLoading: $isLoading, onMessageReceived: { message in
                        // our callback function to let you know of any real-time changes and user activity
                        switch message {
                            
                        case .kinestexLaunched(let data):
                            print("KinesteX Launched: \(data)")
                        case .finishedWorkout(let data):
                            print("Workout Finished: \(data)")
                            // Handle other cases as needed
                        case .exitApp(let data):
                             // user wants to close KinesteX view, so dismiss the view
                            dismiss()
                        default:
                            break
                        }
                        
                    })

                       // OPTIONAL: Display loading screen
                      .overlay(
                        
                        Group {
                            if showAnimation {
                                 Text("Aifying workouts...").foregroundColor(.black).font(.caption)
                                    .frame(maxWidth: .infinity, maxHeight: .infinity) // Fullscreen
                                    .background(Color.white) // White background
                                     .scaleEffect(showAnimation ? 1 : 3) // Scale up
                                    .opacity(showAnimation ? 1 : 0) // Fade out
                                    .animation(.easeInOut(duration: 1.5), value: showAnimation)
                                
                            }
                        }
                        
                        
                    )
                    // Smoothly hide the animation
                   .onChange(of: isLoading) { newValue in
                        if !newValue {
                            withAnimation(.easeInOut(duration: 2.5)) { // Extended duration to 2.5 seconds
                                showAnimation = false
                            }
                            
                        } else {
                            showAnimation = true
                        }
                    }
   
   ```
3. **Handling the data**:
   `onMessageReceived` is a callback function that passes `enum WebViewMessage`
   Available options are: 

```

    kinestexLaunched(String) - Logs when the KinesteX View is successfully launched.
    finishedWorkout(String) - Logs when a workout is finished.
    errorOccurred(String) - Logs when an error has occurred, example (user did not grant access to the camera)
    exerciseCompleted(String) - Logs when an exercise is completed.
    exitApp(String) - Logs when user clicks on exit button and wishes to close the KinesteX view.
    workoutOpened(String) - Logs when the workout description view is opened.
    workoutStarted(String) - Logs when a workout is started.
    planUnlocked(String) - Logs when a plan is unlocked.
    unknown(String) - For handling any unrecognized messages

```

Any questions? Contact us at support@kinestex.com
