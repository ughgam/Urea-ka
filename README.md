# Urea-ka
yes, the title plays with "Eureka" :)

## Installation and Setup Guide

This guide walks you through setting up a Google Cloud project, authenticating Google Earth Engine, and running the interactive Variable-Rate Nitrogen (VRN) simulator on your local machine.


### Step 1: Google Cloud & Earth Engine Setup

Google Earth Engine requires a Google Cloud Project with the Earth Engine API enabled. Follow these steps carefully:

#### 1. Create a Google Cloud Project

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your Google account.
3. Click the project dropdown in the top-left corner (next to the Google Cloud logo) and select **New Project**.
4. Name your project and click **Create**.
5. Note down your **Project ID** (you will need this inside the code where it says `ee.Initialize(project='your-project-id')`).

#### 2. Enable the Earth Engine API

1. In the Cloud Console, look at the top search bar and search for **Google Earth Engine API**.
2. Click on the result under Marketplace/APIs.
3. Click the blue **Enable** button.

#### 3. Register your Project for Earth Engine Access

1. Go to the [Earth Engine Cloud Project Registration Page](https://code.earthengine.google.com/register).
2. Select **Register a non-commercial Cloud Project** (or choose the student/academic option if applicable).
3. Select the Cloud Project ID you created in the steps above.
4. Click **Submit**. It usually happens instantly.

---

### Step 2: Local Project Setup (or instead just use google Colab... no need for installations or anything)

Open your terminal (macOS/Linux) or Command Prompt/PowerShell (Windows) and run the following commands:

#### 1. Clone the Repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_FOLDER_NAME>

```

#### 2. Create a Virtual Environment (just a recommendation)

A virtual environment keeps this project's packages isolated from the rest of your computer.

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate

```

#### 3. Install the Required Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt

```

---

### Step 3: Launching the Application

Since the application uses an interactive map and sliders (`ipywidgets`), it must be run inside a Jupyter environment.

1. Launch Jupyter Lab or Notebook from your terminal:
```bash
jupyter notebook

```


2. Your browser will open a new tab showing your project directory. Click on obvious file name ending with .ipynb to open it.

---

### Step 4: First-Time Authentication & Run Instructions

When you run the notebook for the very first time, Earth Engine needs permission to access your cloud account from your computer.

#### 1. Update your Project ID

Before running, look at the top section of the code and change the project name to your actual Google Cloud Project ID:

```python
ee.Initialize(project='YOUR_ACTUAL_PROJECT_ID_HERE')

```

#### 2. Authenticate

1. In Jupyter, click **Cell** -> **Run All** (or click the first cell and press `Shift + Enter`).
2. The code will pause at the initialization phase and generate a URL link in the output window along with an empty box asking for a verification code.
3. Click the link. It will open a Google authentication page.
4. Log in, check the permissions boxes, and click **Generate Token**.
5. Copy the long authorization code provided on the screen.
6. Switch back to your Jupyter Notebook, paste the code into the verification box, and press **Enter**.

---

### How to Use the Map Interface

Once the script runs completely, the interactive map layout will appear. Follow these operational steps to get your first calculation:

1. **Find your target location:** Use the search bar in the control panel on the right side. Type a location (e.g., "Dhanbad, Jharkhand" or any Pin-Code (OR ZIP code for USA or postal code for Japan) or specific coordinates and click **Teleport**.
2. **Draw your field boundary:** Look at the left edge of the interactive map. Click on the **Rectangle dra tool** (click and drag to a reasonable size) or **Polygon Draw Tool** (the icon shaped like a pentagon and click points on the map to draw a boundary around a crop field, double-click to complete the shape).
3. **Set Parameters:** Adjust the time window slider (default 30 days) and set your target fertilizer rates using the sliders.
4. **Process:** Click **Generate Prescription**. The backend will clip the Sentinel-2 imagery, remove clouds, train the K-Means algorithm, map out management zones, and generate your custom application chart directly below the map interface.

---

### Troubleshooting & Common Pitfalls

> ⚠️ **Error: "No polygon drawn..."**
> Make sure you draw a shape using the map tools on the left side before hitting the execute button. The algorithm needs coordinates to fetch satellite data.

> ⚠️ **Error: "Project not registered..."**
> Make sure you completed Step 1.3. Even if your Google Cloud project is active, Earth Engine requires explicit project registration before it accepts API requests.

> ⚠️ **The Map appears completely black or shows no zones:**
> This happens if the field is covered by clouds during your selected time window or if there are no growing crops there. Try increasing the **Scan Window (Days)** slider to 60 or 90 days to look further back in time for a clear day.
