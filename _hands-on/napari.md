## Running Napari on LUMI Using Singularity

[Napari](https://github.com/napari/napari) is a fast and interactive viewer designed for a wide range of image analysis tasks. The easiest way to launch Napari is to use a containerized version supplied by the NAPARI developers and open it via  Open OnDemand (OoD) web interface. Here are the minimal instructions to open NAPARI app: 

### Step 1: Build the Singularity Container on LUMI

Navigate to the scratch area of your LUMI project and run the following command to build a Napari singularity image:

```bash
singularity build napari.sif docker://ghcr.io/napari/napari-xpra
```
The above command should create a singularity image file napari.sif once the build porcessing is successful.

### Step 2: Move the Image to the Desktop

Once the image is built, move it to your the Desktop folder in your home directory:

```bash
cp napari.sif /users/$USER/Desktop
```
Create a folder "Desktop" if not already exists. Desktop folder is the landing page once "Desktop" app from OoD is launched. 

### Step 3: Launch the LUMI Desktop App

Open the "Desktop" App on the landing page via the [LUMI web interface](https://www.lumi.csc.fi/public/). Fill in the required fields in the submission form for Desktop app. 
For the partition field, select "lumid" for accelerated visualisation. Puhti has an app named "Accelerated visualization" which is directly suitable for NAPARi

### Step 4: Run Napari from the Terminal

In the terminal (the menu bar in the bottom-left corner of the Desktop App) of the LUMI Desktop session, run:

```bash
singularity run napari.sif
```
>**Note** As of writing this, the terminal is available from the menu bar in the bottom-left corner of the Desktop App
### Step 5: Open Napari in Firefox

Open Firefox browser available on LUMI Desktop (**NOT on your local computer**) and navigate to the URL:

```bash
https://localhost:9876
```

>**Note** As of writing this, Firefox browser is available from the menu bar in the bottom-left corner of the Desktop App.
