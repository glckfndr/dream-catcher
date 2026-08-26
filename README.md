# Dream Journal App

A full-stack web application for recording dreams and generating AI-powered interpretations.

## Features

- Record dreams with timestamp
- Get AI interpretations of your dreams
- View all past dreams and their interpretations
- Delete dreams
- SQLite database for persistent storage
- Vanilla JavaScript frontend
- Express backend with RESTful API

## Tech Stack

- **Backend**: Node.js, Express
- **Database**: SQLite (`sqlite` + `sqlite3`)
- **Frontend**: HTML, CSS, Vanilla JavaScript
- **AI**: OpenAI API or Google Gemini API

## Project Structure

```
dream-journal/
├── server.js            # Express server and app bootstrap
├── package.json         # Dependencies and scripts
├── Dockerfile           # Container build config
├── config/
│   ├── database.js      # DB connection
│   └── database-init.js # DB schema initialization
├── routes/
│   └── dreams.js        # REST API endpoints
├── utils/
│   ├── ai-openai.js     # OpenAI integration
│   ├── ai-gemini.js     # Gemini integration
│   └── validateText.js  # Input validation
├── dreams.db            # SQLite database (auto-created)
└── public/
    ├── index.html      # Frontend HTML
    ├── styles.css      # Styles
    └── app.js          # Frontend JavaScript
```

## Setup Instructions

### 1. Install Dependencies

```bash
npm install
```

### 2. Set Up Environment Variables

Create a `.env` file in the root directory and add the variables you need:

```env
# Server
PORT=3001

# Use OpenAI
OPENAI_API_KEY=your_openai_api_key
OPENAI_MODEL=gpt-4o-mini

# Or use Gemini
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-2.5-flash

# Optional DB path override
# DATABASE_PATH=./dreams.db
```

You only need one AI provider key at runtime, based on which integration you use.

### 3. Run the Application

Development mode (with auto-restart):

```bash
npm run dev
```

Production mode:

```bash
npm start
```

The app will be available at `http://localhost:3001` by default.

## API Endpoints

- `GET /api/dreams` - Get all dreams
- `GET /api/dreams/:id` - Get a specific dream
- `POST /api/dreams` - Create a new dream (requires `dream_text` in body)
- `DELETE /api/dreams/:id` - Delete a dream

## Deployment to Render (Docker)

This project is deployed as a Docker image from Docker Hub.

### 1. Build and push image

```bash
docker build --platform linux/amd64 -t glckfndr/dream-journal:v1 .
docker push glckfndr/dream-journal:v1
```

For the next release, increment the tag (for example: `v2`, `v3`).

### 2. Create Web Service in Render

1. Go to https://render.com and sign in.
2. Click **New +** -> **Web Service**.
3. Choose **Deploy an existing image from a registry**.
4. Set image to `glckfndr/dream-journal:v1`.
5. Set **Port** to `3001`.

### 3. Environment variables

Add the variables you actually use:

- `OPENAI_API_KEY` (if using OpenAI)
- `OPENAI_MODEL` (optional, default in code is `gpt-4o-mini`)
- `GEMINI_API_KEY` (if using Gemini)
- `GEMINI_MODEL` (optional, default in code is `gemini-2.5-flash`)
- `DATABASE_PATH=/var/data/dreams.db`

### 4. Persistent storage for SQLite

Attach a **Persistent Disk** in Render and set mount path to `/var/data`.
Without this, SQLite data is lost after restarts/redeploys.

### 5. Deploy updates

1. Build and push a new tag:
   - `docker build --platform linux/amd64 -t glckfndr/dream-journal:v2 .`
   - `docker push glckfndr/dream-journal:v2`
2. In Render, deploy the new image tag (`v2`).

## Usage

1. Enter your dream in the text area
2. Click "Get Interpretation" to save the dream and receive an AI interpretation
3. View all your dreams below, sorted by most recent
4. Click "Delete" to remove a dream

## License

MIT
