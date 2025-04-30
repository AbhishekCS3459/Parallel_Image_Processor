# 🖼️ Parallel Image Processing Service with Java & Spring Boot 🚀

This project demonstrates how to build a **high-performance image processing service** using **Java**, **Spring Boot**, and **Spring Data JPA**. It applies image filters in **parallel** to maximize performance and efficiency.

Client applications can upload an image and request that a specific filter be applied to it using its image ID.

---

## 🔧 Features

- Upload and store images in a database
- Apply image filters like grayscale and brightness
- Parallel image processing using multithreading
- Split and merge image segments for distributed filter application
- RESTful API endpoints for interaction

---

## 🏁 Getting Started

### Prerequisites

- Java 17+
- Maven
- Spring Boot
- Any SQL DB (e.g., MySQL, PostgreSQL)

### Clone and Build

```bash
git clone https://github.com/your-username/parallel-image-processor.git
cd parallel-image-processor
mvn clean install
```

---

## 🧱 Architecture Overview

### 📦 Image Entity

The image is stored in the database as a byte array along with metadata:

```java
@Entity
public class Image {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Lob
    @Column(columnDefinition = "LONGBLOB")
    private byte[] imageData;

    private String filename;
    private String contentType;
}
```

---

### 🎨 Filter Implementation

Filters are implemented as beans and conform to a common `Filter` interface:

```java
@Component
public class GrayScaleFilter implements Filter {
    @Override
    public BufferedImage applyFilter(BufferedImage image) {
        // Convert image to grayscale
    }
}
```

---

### 🔗 REST API Endpoints

#### Upload Image

```http
POST /upload
```
**Request**: `multipart/form-data` with image file  
**Response**: `imageId` (Long)

#### Apply Filter

```http
GET /filter?imageId={id}&filterType={filter}
```
**Response**: Base64 string of the filtered image

---

## ⚙️ Parallel Processing Logic

The image is split into segments and each segment is processed concurrently:

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
for (int i = 0; i < segments.length; i++) {
    int index = i;
    executor.execute(() -> processedSegments[index] = filter.applyFilter(segments[index]));
}
executor.shutdown();
executor.awaitTermination(...);
```

---

### 🧩 Image Splitting & Merging

#### Split Image

```java
public static BufferedImage[] splitImage(BufferedImage image, int rows, int cols)
```

#### Merge Image

```java
public static BufferedImage mergeImageParts(BufferedImage[] parts, int rows, int cols, boolean isGray)
```

---

## 📁 Project Structure

```
├── controller/
│   └── ImageController.java
├── service/
│   └── ImageProcessingService.java
├── model/
│   └── Image.java
├── filters/
│   ├── Filter.java
│   └── GrayScaleFilter.java
├── util/
│   └── ImageUtils.java
```

---

## 📸 Sample Request

```bash
curl -F "file=@/path/to/image.jpg" http://localhost:8080/upload
curl "http://localhost:8080/filter?imageId=1&filterType=grayscale"
```

---

## 🧠 Concepts Covered

- Multithreaded programming in Java
- Image processing with BufferedImage
- RESTful API design
- Spring Boot & Spring Data JPA integration
- Base64 encoding for image responses

---

## 🔗 Repository

📂 You can find the full source code in this GitHub repository:  
👉 [GitHub Repo Link](https://github.com/AbhishekCS3459/Parallel_Image_Processor)

---

## 🙌 Like this project?

If you found this helpful, give it a ⭐ on GitHub or share it with your peers. Contributions are welcome!

---

Would you like me to generate a `logo.png` or architecture diagram for this README as well?
