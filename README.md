# 3D Animated Airplane Model using OpenGL & GLUT ✈️🌐

An advanced computer graphics project developed in C++ using the **OpenGL** and **GLUT** libraries to design, render, and animate a structured three-dimensional airplane model.

---

## 📌 Project Overview
This project applies foundational concepts of 3D computer graphics to construct a detailed aircraft model. The architecture divides the airplane into distinct components—the fuselage, wings, and tail system—built entirely using standard geometric primitives. The system leverages hierarchical modeling to apply independent mathematical transformations to each component while maintaining a unified global animation loop.

### Key Features:
- **Hierarchical 3D Modeling:** Utilizing primitives such as cylinders, cones, and cubes to assemble a complex structural vehicle.
- **Matrix Transformations:** Implementing precise execution of translation (`glTranslatef`), rotation (`glRotatef`), and scaling (`glScalef`) matrices.
- **Real-Time Animation:** Utilizing GLUT timer functions to achieve smooth, continuous $360^\circ$ rotation within a 3D coordinate space.
- **Depth Testing & Rendering:** Enabling `GL_DEPTH_TEST` to ensure accurate hidden-surface removal and realistic perspective drawing.

---

## 🛠️ Tech Stack & Prerequisites
- **Programming Language:** C++
- **Graphics APIs:** OpenGL, GLU (OpenGL Utility Library)
- **Window Management:** GLUT (OpenGL Utility Toolkit)
- **Development Environment:** Compatible with modern C++ compilers setup with OpenGL configurations (e.g., Visual Studio, Code::Blocks).

---

## ⚙️ Core Architectural Components

### 1. The Fuselage (Body)
Rendered dynamically using a custom quadric cylinder combined with a forward-facing cone (`glutSolidCone`) to shape the nose of the aircraft, colored in high-contrast red and gray tones.

### 2. Wing Assemblies
Constructed using standardized cubes scaled down into thin, wide horizontal plates positioned symmetrically along the lateral axes.

### 3. Tail System
Comprises horizontal stabilizers and a vertical fin assembled from transformed bounding cubes to control spatial equilibrium.

---

## 💻 Code Structure & Graphics Pipeline

The source code organizes the graphics pipeline into isolated rendering routines unified under a main execution loop:

```cpp
#include <GL/glut.h>

float angle = 0.0f; // Global rotation angle

void init() {
    glEnable(GL_DEPTH_TEST); // Enable depth buffer for 3D rendering
    glClearColor(0.7f, 0.9f, 1.0f, 1.0f); // Sky blue background
}

void drawPlane() {
    drawFuselage(); // Renders main body and nose cone
    drawWings();    // Renders primary wings
    drawTail();     // Renders rear tail assembly
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glLoadIdentity();
    
    // Set up camera perspective view
    gluLookAt(4.0, 2.5, 5.0,  1.0, 0.0, 0.0,  0.0, 1.0, 0.0);
    
    glRotatef(angle, 0.0, 1.0, 0.0); // Apply real-time rotation
    drawPlane();
    
    glutSwapBuffers(); // Swap front/back buffers for smooth animation
}

void timer(int value) {
    angle += 0.5f; // Incremental rotation step
    if (angle > 360.0f) angle -= 360.0f;
    
    glutPostRedisplay(); // Request window redraw
    glutTimerFunc(16, timer, 0); // ~60 Frames Per Second (FPS) loop
}
