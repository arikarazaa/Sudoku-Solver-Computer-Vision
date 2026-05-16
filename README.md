# Sudoku-Solver-Computer-Vision
Sudoku Solver — Computer Vision
A computer vision pipeline that reads a Sudoku puzzle from a photo, recognizes digits using a CNN trained on MNIST, and overlays the solution back onto the original image. Built with OpenCV, TensorFlow/Keras, and a backtracking solver — runs end-to-end in Google Colab.

Demo
Input photoExtracted gridSolved outputRaw puzzle imagePerspective-corrected top-down viewGreen digits filled in on the warped grid

Pipeline overview
Photo → Preprocessing → Grid detection → Perspective warp
     → Cell extraction → Digit recognition (CNN) → Backtracking solver
     → Solution overlay → Output image

Preprocessing — Gaussian blur + adaptive thresholding to isolate grid lines
Grid detection — Contour detection to find the largest quadrilateral (the board)
Perspective warp — Four-point transform to get a clean top-down view
Cell extraction — Split warped grid into 81 individual cell images
Digit recognition — Small CNN trained on MNIST classifies each cell (1–9 or empty)
Solving — Recursive backtracking algorithm fills in all empty cells
Overlay — Solved digits drawn back onto the warped image in green


Tech stack
LibraryPurposeOpenCVImage preprocessing, contour detection, perspective warpimutilsContour utilities, four-point transform helperscikit-imageclear_border() to strip grid-line artifacts from cellsTensorFlow / KerasCNN digit classifier trained on MNISTNumPyBoard representation and array operationsMatplotlibDebug visualization (9×9 cell preview grid)

Getting started
Platform: Google Colab (recommended) — no local setup needed.

Open SudokoSolver.ipynb in Google Colab
Run Cell 1 to install imutils
Restart the runtime if prompted
Run all cells top to bottom
When prompted, upload a clear photo of your Sudoku puzzle
The solved image will download automatically at the end

Local setup (optional):
bashpip install opencv-python imutils scikit-image tensorflow numpy matplotlib
jupyter notebook SudokoSolver.ipynb

Project structure
sudoku-solver/
├── SudokoSolver.ipynb   # Main notebook — all stages in order
├── digit_model.h5       # Saved CNN weights (generated on first run)
├── solved_sudoku.jpg    # Output image (generated after solving)
└── README.md

digit_model.h5 is created automatically on first run and reused on subsequent runs to avoid retraining.


Model details

Architecture: 2× Conv2D + MaxPooling → Flatten → Dense(128) → Dropout(0.5) → Dense(10, softmax)
Dataset: MNIST handwritten digits (60,000 train / 10,000 test)
Training: 5 epochs, batch size 128, Adam optimizer
Accuracy: ~99% on MNIST test set
Confidence threshold: 0.75 — cells below this are treated as empty


Note: MNIST consists of handwritten digits. Printed Sudoku puzzles use clean typeset fonts, which can cause occasional misreads. If recognition fails, use the manual override cell in the notebook.


Known limitations

Works best on flat, well-lit images with a clear grid border
Digit recognition is trained on handwritten MNIST — printed fonts may occasionally misclassify
Perspective correction requires the full grid to be visible in the frame
Solution is overlaid on the warped (flattened) image, not projected back onto the original photo


Possible improvements

Fine-tune on a printed digit dataset for better accuracy on real puzzles
Inverse perspective warp to project the solution back onto the original photo
Multiple puzzle detection (solve several puzzles in one image)
Web or mobile interface


License
MIT
