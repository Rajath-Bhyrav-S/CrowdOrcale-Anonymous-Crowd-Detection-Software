# CrowdOracle Frontend

A beautiful, modern frontend for the CrowdOracle crowd detection system. Uses TensorFlow.js with COCO-SSD model for real-time person detection.

## Features

- 📷 **Live Camera Feed** - Access your device's camera for real-time monitoring
- 🤖 **AI-Powered Detection** - Uses TensorFlow.js COCO-SSD model to detect people
- 📊 **Real-time Statistics** - Live count of people with capacity indicators
- 🌡️ **Temperature Input** - Manual temperature entry for environmental data
- 📈 **Capacity Monitoring** - Visual indicators when room is getting crowded
- 🔄 **Auto-sync** - Automatically sends data to backend every 5 seconds
- ⚙️ **Configurable Settings** - Customize update interval, confidence threshold, etc.
- 📸 **Screenshots** - Capture moments with detection overlays

## Quick Start

### Option 1: Using Live Server (VS Code Extension)

1. Install the "Live Server" extension in VS Code
2. Right-click on `index.html` and select "Open with Live Server"
3. The frontend will open at `http://localhost:5500`

### Option 2: Using Python HTTP Server

```bash
cd frontend
python -m http.server 3000
```

Then open `http://localhost:3000` in your browser.

### Option 3: Using Node.js HTTP Server

```bash
npx serve frontend -l 3000
```

Then open `http://localhost:3000` in your browser.

## Backend Setup

Make sure your Spring Boot backend is running on `http://localhost:8080`. The frontend will automatically connect to it.

### Starting the Backend

```bash
cd Backend
./mvnw spring-boot:run
```

Or on Windows:

```bash
cd Backend
mvnw.cmd spring-boot:run
```

## How It Works

1. **Camera Access** - Click "Start Camera" to allow camera access
2. **AI Detection** - The COCO-SSD model detects people in the video feed
3. **Counting** - People are counted and displayed with bounding boxes
4. **Data Upload** - Every 5 seconds (configurable), the current count and temperature are sent to the backend
5. **Capacity Alerts** - Visual indicators show when room is getting crowded

## API Integration

The frontend sends data to the backend in this format:

```json
POST /api/crowd-data
{
    "temperatureCelsius": 25.0,
    "totalPeopleCount": 15
}
```

## Settings

Click the ⚙️ Settings button to configure:

- **Backend URL** - Where to send data (default: `http://localhost:8080`)
- **Update Interval** - How often to send data (default: 5 seconds)
- **Confidence Threshold** - Minimum confidence for detection (default: 0.5)
- **Auto-send Data** - Enable/disable automatic data sending
- **Show Bounding Boxes** - Toggle detection overlay visibility

## Browser Compatibility

Works best on:
- Chrome (recommended)
- Firefox
- Edge
- Safari (with camera permissions)

## Troubleshooting

### Camera not working?
- Make sure you've granted camera permissions
- Try using HTTPS (required by some browsers)
- Check if another application is using the camera

### Connection errors?
- Verify the backend is running on `http://localhost:8080`
- Check CORS settings if using a different port
- Look at browser console for detailed error messages

### Detection not accurate?
- Ensure good lighting conditions
- Adjust the confidence threshold in settings
- Position the camera for a clear view

## Technologies Used

- **TensorFlow.js** - Machine learning in the browser
- **COCO-SSD** - Pre-trained object detection model
- **Font Awesome** - Icons
- **Google Fonts (Inter)** - Typography
