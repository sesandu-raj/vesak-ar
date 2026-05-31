# 🪔 Vesak AR Lantern

A simple WebAR project that displays a glowing 3D Vesak lantern using **A-Frame** and **AR.js**. Users can point their camera at the Hiro marker and view an animated lantern directly in their mobile browser without installing any application.

## Features

* Web-based Augmented Reality (WebAR)
* No app installation required
* Marker-based AR using the Hiro marker
* Animated 3D Vesak lantern
* Real-time camera tracking
* Mobile browser compatible

## Technologies Used

* HTML5
* CSS3
* JavaScript
* A-Frame
* AR.js

## Getting Started

### Clone the Repository

```bash
git clone https://github.com/your-username/vesak-ar-lantern.git
cd vesak-ar-lantern
```

### Run Locally

Start a local web server:

```bash
python3 -m http.server 8000
```

Open:

```text
http://localhost:8000
```

### Testing

1. Open the website on a mobile device.
2. Allow camera permissions.
3. Point the camera at a Hiro marker.
4. The animated Vesak lantern will appear above the marker.

## Hiro Marker

You can use the default Hiro marker provided by AR.js:

https://raw.githubusercontent.com/AR-js-org/AR.js/master/data/images/hiro.png

Print it or display it on another screen for testing.

---

## Important Branch Information

⚠️ **This `main` branch contains the basic marker-based AR prototype only.**

The version currently deployed and hosted online is available in the **`import3D`** branch.

The `import3D` branch includes:

* Imported 3D models (.glb)
* Enhanced Vesak lantern visuals
* Improved AR experience
* Production-ready implementation

To view the hosted version's source code, switch to:

```bash
git checkout import3D
```

or select the **import3D** branch from the GitHub branch selector.

---

## Project Vision

This project explores how traditional Sri Lankan Vesak celebrations can be combined with modern Augmented Reality technologies to create an engaging digital cultural experience.

## Authors

Developed as a Computer Science student project for Vesak celebrations and WebAR experimentation.
