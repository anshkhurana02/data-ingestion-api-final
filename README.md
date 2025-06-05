
# ⚙️ Data Ingestion API

A production-ready, rate-limited Data Ingestion API built using **Node.js** and **Express.js**. This API accepts a list of IDs, splits them into asynchronous processing batches, tracks the processing status, and enforces per-IP rate limits to ensure reliability and scalability.

---

## 🚀 Live Demo

> **Deployed at:** [https://your-app-name.up.railway.app](https://your-app-name.up.railway.app)  
> *(Replace with your actual deployed URL)*

---

## 📂 Project Structure

```
data_ingestion_api/
├── index.js              # Entry point
├── routes/               # API route definitions
├── controllers/          # Business logic for each endpoint
├── services/             # Async batch processing and status tracking
├── middlewares/          # Rate limiter and body parser
├── tests/                # Endpoint tests with Supertest and Jest
├── package.json
└── README.md
```

---

## 🛠️ Technologies Used

- **Node.js**
- **Express.js**
- **UUID** – For unique ingestion IDs
- **Rate Limiter** – Express middleware
- **Jest & Supertest** – Testing framework

---

## 🧠 Design Choices

| Feature             | Implementation |
|---------------------|----------------|
| **Batching**         | Dynamically splits input IDs into configurable batch sizes. |
| **Asynchronous Processing** | Batches are processed in the background with `setTimeout` (can be replaced with a queue or worker). |
| **Status Tracking**  | Each ingestion and its batches are stored in-memory with real-time status updates. |
| **Rate Limiting**    | Per-IP rate limiting using `express-rate-limit`. |
| **Separation of Concerns** | Follows MVC-style file structure for maintainability. |

---

## 🔧 Local Setup

### ✅ Prerequisites

- Node.js 
- npm

### 📦 Install Dependencies

```bash
npm install
```

### ▶️ Run Server

```bash
npm start
```

Server starts on `http://localhost:5000`

---

## 📬 API Endpoints

### 1. **POST /ingest**

Submit an ingestion request with an array of IDs.

#### Request:

```json
{
  "ids": [1, 2, 3, 4, 5]
}
```

#### Sample CURL:

```bash
curl -X POST http://localhost:5000/ingest   -H "Content-Type: application/json"   -d '{"ids": [1, 2, 3, 4, 5]}'
```

#### Response:

```json
{
  "ingestion_id": "abc123"
}
```

---

### 2. **GET /status/:ingestion_id**

Fetch the status of a previously submitted ingestion.

#### Sample CURL:

```bash
curl http://localhost:5000/status/abc123
```

#### Response:

```json
{
  "ingestion_id": "abc123",
  "status": "processing",
  "batches": [
    { "batch_id": 1, "ids": [1, 2, 3], "status": "completed" },
    { "batch_id": 2, "ids": [4, 5], "status": "processing" }
  ]
}
```

---

## ⏱️ Rate Limiting

This API restricts usage to **10 requests per minute per IP address**.

- If the limit is exceeded, it returns:
  ```json
  {
    "message": "Too many requests. Please try again later."
  }
  ```

---

## ✅ Testing

Run the test suite:

```bash
npm test
```

Test coverage includes:

- ✔️ Successful ingestion
- ✔️ Status retrieval
- ✔️ Batch correctness
- ✔️ Invalid request handling
- ✔️ Rate limiting enforcement

Tests are located in the `/tests` folder and use [**Supertest**](https://www.npmjs.com/package/supertest) with [**Jest**](https://jestjs.io/).

---

## 🌍 Deployment

You can deploy this API using platforms like:

- [Railway.app](https://railway.app)
- [Render.com](https://render.com)
- [Heroku](https://heroku.com)

> Ensure your `index.js` uses dynamic port binding:
```js
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

## 📘 Example Flow

1. Client POSTs `ids` → `/ingest`
2. Server responds with `ingestion_id`
3. Server splits IDs into batches (size configurable)
4. Server starts processing batches asynchronously
5. Client polls `/status/:ingestion_id` for real-time updates

---

## 📜 License

MIT License

---

## 🙋‍♂️ Author

**Ansh Khurana**  
📫 [GitHub](https://github.com/your-username)  
