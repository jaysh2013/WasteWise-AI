# WasteWise AI

> **Know where your waste belongs before you throw it.**

WasteWise AI is a browser-based AI waste-identification prototype
created by **Team BLAZE** as a hackathon project focused on **AI ×
Sustainability**.

The application lets a user either turn on their device camera or upload
a photo of a waste item. A pretrained computer-vision model analyzes the
image, identifies the most likely object, and passes that model label
through a pattern-based waste knowledge layer. The application then
provides a practical recommendation covering the item's waste category,
disposal method, reuse ideas, environmental risk, and---when
applicable---AI confidence.

The prototype is designed around a simple principle:

**Image → Vision AI → Waste ID → Material/Category → Decision →
Recommendation**

The website is implemented as a self-contained HTML page with CSS and
JavaScript. The live scanner loads TensorFlow.js and the MobileNet model
from public CDNs when the scanner is used.

------------------------------------------------------------------------

## 1. Project Overview

Correct waste segregation is often difficult because identifying an
object is not the same as knowing how it should be disposed of. Items
may contain mixed materials, batteries, food residue, electronics, or
other components that require different handling.

WasteWise AI attempts to solve this problem at the moment a person is
about to throw something away.

### What the application does

1.  Accepts an image from a live camera or an uploaded photo.
2.  Captures/resizes the image in the browser.
3.  Loads a pretrained MobileNet computer-vision model.
4.  Uses the model to classify the image into one of its general object
    categories.
5.  Applies a rule-based knowledge layer to the model's returned label.
6.  Determines an appropriate waste category and disposal
    recommendation.
7.  Displays reuse ideas and environmental-risk information.
8.  Rejects low-confidence predictions instead of pretending to know the
    answer.
9.  Provides manual description as a fallback when image recognition is
    insufficient.

The website also contains an **Example Items** mode for demonstrating
predefined waste scenarios such as plastic bottles, batteries, food
wrappers, and glass jars.

------------------------------------------------------------------------

## 2. Key Features

### AI Waste Scanner

-   Live camera scanning using the browser camera API.
-   Upload-an-image option.
-   Captures a single item per scan.
-   Uses a genuine pretrained computer-vision model rather than only a
    fixed list of image examples.
-   Displays an AI confidence percentage for image-based predictions.

### Waste Categorization

The knowledge layer contains pattern-based rules for many types of
objects, including:

-   Batteries
-   Medical/sharp waste
-   Smartphones
-   Computers and other electronics
-   Headphones and small electronics
-   Chargers and cables
-   Large appliances
-   Light bulbs
-   Bottles and jars
-   Metal cans
-   Cardboard and packaging
-   Paper
-   Organic/food waste
-   Plastic bags and wrappers
-   Plastic containers
-   Diapers
-   Medicine packaging
-   Clothing/textiles
-   Footwear
-   Bags and accessories
-   Umbrellas
-   Toys
-   Furniture
-   Kitchenware

### Disposal Guidance

Results can include practical instructions such as:

-   Dry-waste/recycling stream
-   Wet/organic waste
-   E-waste collection
-   Battery collection
-   Medical-waste collection
-   General waste
-   Bulky-waste pickup
-   Donation or textile-recycling options
-   Local guidance checks for materials whose recycling rules vary

### Environmental Risk

Each recognized category can be assigned a risk level:

-   **Low**
-   **Medium**
-   **High**

The interface also explains why an item may be risky.

### Reuse Suggestions

Instead of immediately treating an item as trash, WasteWise AI can
suggest ways to:

-   Reuse it
-   Donate it
-   Repair it
-   Repurpose it
-   Recycle it appropriately

### Low-Confidence Handling

The live scanner uses a confidence threshold of **0.35**.

If the top prediction is below this threshold, the application does
**not** invent a result. Instead, it offers:

-   Retake photo
-   Upload another image
-   Describe the item manually

This is an important safety/design decision because an uncertain
classification can result in incorrect disposal advice.

### Manual Description Fallback

Users can type a description such as:

-   `plastic shampoo bottle`
-   `old charger`
-   `battery`

The same categorization knowledge layer is then used to provide
guidance. Manual results are explicitly marked as being based on the
user's description rather than verified by image AI.

### Responsive Interface

The website includes:

-   Responsive desktop/mobile layouts
-   Mobile navigation menu
-   Live camera controls
-   Front/rear camera switching where multiple cameras are available
-   Keyboard-accessible controls
-   Reduced-motion support
-   Focus-visible accessibility styling

------------------------------------------------------------------------

## 3. How the AI Pipeline Works

WasteWise AI uses two distinct layers.

### Layer 1 --- Computer Vision

The live scanner dynamically loads:

-   **TensorFlow.js 4.20.0**
-   **MobileNet 2.1.1**

MobileNet is loaded with:

``` javascript
mobilenet.load({ version: 2, alpha: 1.0 })
```

The model returns the top five classifications, and the application uses
the highest-probability prediction.

Conceptually:

``` text
Photo
  ↓
Canvas image
  ↓
MobileNet
  ↓
Top object label + probability
```

The implementation describes MobileNet as a pretrained model covering
roughly 1,000 general object categories.

### Layer 2 --- Waste Knowledge / Decision Engine

The raw model label is not directly treated as a disposal instruction.

Instead, the label is passed through a pattern-based rules engine.

For example, multiple labels containing terms related to bottles or jars
can match the same waste rule.

Conceptually:

``` text
MobileNet label
      ↓
Regex / pattern matching
      ↓
Waste category
      ↓
Disposal guidance
      ↓
Risk + reuse recommendations
```

This separates **object recognition** from **waste-management
reasoning**.

### Generic Fallback

If no rule matches the model's label, the application creates a generic
result:

-   Category: `General item`
-   Tag: `Uncatalogued`
-   Risk: `Medium`

It then tells the user to check local waste guidelines rather than
pretending that the item has a known disposal route.

------------------------------------------------------------------------

## 4. AI Tools / Models Used

  -----------------------------------------------------------------------
  AI / Tool                           Purpose
  ----------------------------------- -----------------------------------
  **MobileNet 2.1.1**                 Pretrained image classification for
                                      identifying the photographed object

  **TensorFlow.js 4.20.0**            Runs the computer-vision model
                                      directly in the browser

  **Pattern-based rules engine**      Maps recognized object labels to
                                      waste categories, disposal
                                      instructions, risk levels, and
                                      reuse ideas

  **Confidence thresholding**         Prevents the application from
                                      presenting low-confidence image
                                      classifications as reliable answers
  -----------------------------------------------------------------------

### Important clarification

The provided prototype does **not** use a generative AI/LLM such as GPT,
Claude, Gemini, or a vision-language API.

It uses a pretrained **MobileNet computer-vision classifier** plus a
deterministic JavaScript rules/knowledge layer.

The website's informational section contains a placeholder for the exact
AI model, but the implemented live-camera code explicitly loads
**TensorFlow.js + MobileNet**.

------------------------------------------------------------------------

## 5. Technologies Used

### Frontend

-   HTML5
-   CSS3
-   Vanilla JavaScript
-   SVG icons
-   Responsive CSS/Grid/Flexbox

### Browser APIs

-   `navigator.mediaDevices.getUserMedia()` for camera access
-   Canvas API for image capture and resizing
-   File input API for uploaded images
-   DOM APIs for UI updates
-   `requestAnimationFrame()` for confidence-bar animation

### Machine Learning

-   TensorFlow.js
-   MobileNet

### External Assets

-   Google Fonts:
    -   Bricolage Grotesque
    -   Inter

### CDN Dependencies

The live scanner loads:

``` text
https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@4.20.0/dist/tf.min.js
https://cdn.jsdelivr.net/npm/@tensorflow-models/mobilenet@2.1.1/dist/mobilenet.min.js
```

------------------------------------------------------------------------

## 6. Project Structure

The supplied prototype is currently implemented as a single
self-contained HTML file:

``` text
WasteWise AI/
└── WasteWise_AI_Website.html
```

The HTML file contains three major parts:

``` text
WasteWise_AI_Website.html
│
├── <head>
│   ├── Metadata
│   ├── Page title
│   ├── Google Fonts
│   └── Complete CSS
│
├── <body>
│   ├── Navigation
│   ├── Hero section
│   ├── AI scanner
│   ├── Problem section
│   ├── How-it-works section
│   ├── Differentiation section
│   ├── Features section
│   ├── Impact section
│   └── Footer
│
└── <script>
    ├── Mobile navigation
    ├── Example-item demo
    ├── Waste rules
    ├── Camera handling
    ├── Image upload handling
    ├── TensorFlow.js loading
    ├── MobileNet loading
    ├── Image classification
    ├── Confidence checking
    ├── Waste categorization
    ├── Result rendering
    └── Manual description fallback
```

### Recommended production structure

If the prototype is expanded into a larger application, it would be
better to split it into:

``` text
WasteWise-AI/
├── index.html
├── css/
│   └── styles.css
├── js/
│   ├── app.js
│   ├── camera.js
│   ├── classifier.js
│   ├── wasteRules.js
│   └── ui.js
├── assets/
│   └── icons/
├── README.md
└── LICENSE
```

The above is a recommended future structure, not the current structure
of the supplied prototype.

------------------------------------------------------------------------

## 7. Setup / Installation

### Prerequisites

You need:

-   A modern web browser such as Chrome, Edge, or Safari
-   Internet access when the live AI scanner first loads its CDN
    dependencies
-   Camera permission if using live camera mode

No Python installation, Node.js installation, package manager, or
backend server is required for the supplied single-file prototype.

### Option 1 --- Open Directly

The simplest method is to open:

``` text
WasteWise_AI_Website.html
```

in a modern browser.

The **Example Items** mode can be used without camera access.

### Option 2 --- Run with a Local Web Server

For the best camera compatibility, serve the file through a local web
server.

For example, with Python:

``` bash
python -m http.server 8000
```

Then open:

``` text
http://localhost:8000/WasteWise_AI_Website.html
```

### Camera Requirements

The application uses the browser's camera API:

``` javascript
navigator.mediaDevices.getUserMedia()
```

Camera access may require a secure context such as:

-   `https://`
-   `localhost`

If camera permission is blocked, the application provides an upload
option as an alternative.

------------------------------------------------------------------------

## 8. Usage

### Live Camera

1.  Open the WasteWise AI website.
2.  Select **Live camera**.
3.  Click **Turn on camera**.
4.  Allow camera permission.
5.  Point the camera at one waste item.
6.  Click the capture button.
7.  Wait while the vision model analyzes the image.
8.  Review:
    -   Identified item
    -   Waste category
    -   AI confidence
    -   Disposal recommendation
    -   Reuse ideas
    -   Environmental risk

### Upload a Photo

1.  Select **Live camera**.
2.  Click **Upload a photo**.
3.  Choose an image.
4.  Wait for classification.
5.  Review the recommendation.

### Example Items

1.  Open the **Example items** tab.
2.  Choose an example such as:
    -   Plastic bottle
    -   Battery
    -   Food wrapper
    -   Glass jar
3.  The demo runs an animated scan.
4.  The predefined result is displayed.

### Manual Description

If the AI cannot confidently identify an item:

1.  Click **Describe it manually**.
2.  Enter a description.
3.  Click **Get guidance**.
4.  Review the rule-based recommendation.

Manual results are clearly distinguished from image-AI results.

------------------------------------------------------------------------

## 9. Privacy

The live scanner is designed to process captured images in the browser.

The website states that images are processed fully in the browser and
are not uploaded to a backend service.

The supplied implementation:

-   Uses the browser camera API.
-   Draws captured frames to an HTML canvas.
-   Passes the canvas directly to MobileNet.
-   Does not contain a backend upload endpoint.
-   Does not include an application server or database.

However, the AI model JavaScript libraries themselves are loaded from
public CDNs, so **internet access is required to download the
model/runtime unless the dependencies are later bundled locally**.

------------------------------------------------------------------------

## 10. Waste Rules and Decision Logic

The rule engine uses regular expressions to match groups of possible
MobileNet labels.

Examples include:

``` text
battery|batteries
```

for batteries,

``` text
charger|adapter|cable|power cord|plug
```

for chargers and cables, and:

``` text
banana|apple|orange|lemon|fruit|food waste|leftovers
```

for organic/wet waste.

Each matching rule can define:

-   Item name
-   Waste category
-   Result tag
-   Risk level
-   Reason for the risk
-   Disposal recommendation
-   Reuse ideas

This allows one rule to cover multiple related model labels instead of
maintaining a one-to-one lookup table.

------------------------------------------------------------------------

## 11. Example Decision Categories

The current rules demonstrate several common decisions.

### Battery

**Category:** Battery\
**Result:** Hazardous\
**Risk:** High

Recommendation: use an authorized battery/e-waste collection point
rather than normal household waste.

### Smartphone

**Category:** E-waste\
**Result:** Hazardous\
**Risk:** High

Recommendation: use an authorized e-waste collection or recycling
channel.

### Plastic Bottle / Jar

**Category:** Glass or plastic\
**Result:** Recyclable\
**Risk:** Low

Recommendation: empty and rinse it, then use the appropriate
dry-waste/recycling stream.

### Organic Waste

**Category:** Organic / wet waste\
**Result:** Compostable\
**Risk:** Low

Recommendation: compost it or place it in the appropriate wet/organic
waste stream.

### Plastic Bag

**Category:** Plastic film\
**Result:** Check locally\
**Risk:** Medium

Recommendation: check for a dedicated plastic-film drop-off because
curbside programs often do not accept thin plastic film.

------------------------------------------------------------------------

## 12. Important Limitations

WasteWise AI is a **prototype/hackathon project**, not a certified
waste-management authority.

### Model limitations

MobileNet is a general-purpose image classifier rather than a
waste-specific model. It may identify an object imperfectly, especially
when:

-   The image is blurry.
-   Lighting is poor.
-   Multiple objects are visible.
-   The object is unusual.
-   The object is partially hidden.
-   The object does not correspond well to ImageNet categories.

### Disposal-rule limitations

Waste-management rules vary by:

-   City
-   Country
-   Waste-management provider
-   Material type
-   Local recycling infrastructure

Therefore, the recommendations should be treated as guidance rather than
universal legal or municipal instructions.

The rule engine intentionally uses phrases such as **"check locally"**
for categories where disposal requirements can vary.

### One item per image

The scanner is designed to identify one main item per photo. For
multiple waste items, users should scan them individually.

### Internet dependency

TensorFlow.js and MobileNet are fetched from jsDelivr when required. The
live scanner therefore needs internet access unless the dependencies are
self-hosted.

------------------------------------------------------------------------

## 13. Security and Safety Considerations

The application intentionally avoids presenting low-confidence
classifications as certain.

The confidence threshold is:

``` javascript
const CONFIDENCE_THRESHOLD = 0.35;
```

When the highest-probability prediction is below the threshold, the user
receives a low-confidence message and alternative actions instead of an
automatic classification.

For hazardous categories such as:

-   Batteries
-   Medical/sharp waste
-   E-waste
-   Certain bulbs
-   Medicine-related waste

the interface emphasizes safer disposal channels.

Users should always follow official local guidance for hazardous waste.

------------------------------------------------------------------------

## 14. User Experience Design

The interface is intentionally designed around a quick decision-making
workflow.

### Design principles

-   Minimal steps between photo and recommendation
-   Clear visual distinction between safe/recyclable and hazardous
    results
-   Confidence shown explicitly
-   Reuse presented alongside disposal
-   Low-confidence results handled transparently
-   Mobile-first camera interaction
-   Responsive layout
-   Accessible focus states
-   Reduced-motion support

The main website sections are:

1.  **Problem** --- explains why waste decisions are difficult.
2.  **How it works** --- presents the AI pipeline.
3.  **Features** --- explains the scanner and decision capabilities.
4.  **Impact** --- connects individual disposal decisions to broader
    sustainability outcomes.

------------------------------------------------------------------------

## 15. Current Prototype vs. Future Improvements

### Current prototype

The supplied implementation provides:

-   Browser-based camera scanning
-   Image upload
-   MobileNet image classification
-   Rule-based waste categorization
-   Disposal guidance
-   Risk levels
-   Reuse suggestions
-   Confidence handling
-   Manual fallback
-   Example-item demonstration
-   Responsive UI

### Potential future improvements

For a production-ready version, the project could be extended with:

-   A waste-specific computer-vision model trained on real waste
    datasets
-   Better fine-grained material recognition
-   Multi-object detection
-   Localized disposal rules based on the user's city
-   Municipal recycling-center lookup
-   Multilingual support
-   User feedback and correction loops
-   Model evaluation metrics
-   Offline model support
-   Local model hosting instead of CDN dependencies
-   Progressive Web App support
-   Scan history
-   Analytics with privacy controls
-   Authentication and user profiles
-   Accessibility improvements and testing
-   Automated test coverage
-   CI/CD deployment
-   A dedicated backend/API if future features require server-side
    processing

------------------------------------------------------------------------

## 16. Development Notes

The live AI implementation is intentionally separate from the static
example-item demonstration.

### Example demo

The example mode uses predefined JavaScript objects containing fields
such as:

``` javascript
{
  label,
  category,
  stream,
  tag,
  hazard,
  confidence,
  risk,
  why,
  note,
  reuse
}
```

These values are used to demonstrate the interface.

### Live AI mode

The live mode:

``` text
Camera / Upload
      ↓
Canvas
      ↓
MobileNet
      ↓
Top prediction
      ↓
Confidence threshold
      ↓
Waste rule matching
      ↓
Result rendering
```

This distinction is important when evaluating the prototype: **example
results are predefined demonstrations, while live-camera results are
generated from the MobileNet classification pipeline and then
interpreted by the rules layer.**

------------------------------------------------------------------------

## 17. Team

**Team BLAZE**

Project theme:

**AI × Sustainability**

Project name:

**WasteWise AI**

Tagline:

> **Know where your waste belongs before you throw it.**

------------------------------------------------------------------------

## 18. License

No explicit license is specified in the supplied project file.

If this project is published publicly, add an appropriate `LICENSE` file
and update this section with the chosen license terms.

------------------------------------------------------------------------

## 19. Disclaimer

WasteWise AI is an experimental educational/hackathon prototype.

The application should not be treated as a substitute for official
municipal, recycling-provider, medical-waste, hazardous-waste, or
environmental guidance. When an item is hazardous, uncertain,
contaminated, or not recognized, users should follow local official
disposal instructions.

------------------------------------------------------------------------

## 20. Quick Start

``` bash
# 1. Put the HTML file in your project folder

# 2. Start a simple local server
python -m http.server 8000

# 3. Open the application
# http://localhost:8000/WasteWise_AI_Website.html
```

Then:

``` text
Live camera
    ↓
Capture/upload item
    ↓
MobileNet analyzes image
    ↓
Confidence check
    ↓
Waste rule matching
    ↓
Disposal + risk + reuse guidance
```

**WasteWise AI turns recognition into action.**
