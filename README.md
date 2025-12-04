# AI Chat

A simple chat application powered by Google Gemini AI, built with Vue.js and Vite.

## Features

- Real-time chat interface with Google Gemini AI
- Clean and modern UI design
- Responsive layout
- Dark mode support
- Text-based conversation with AI

## Setup

### Prerequisites

- Node.js (v18 or higher)
- A Google Gemini API key ([Get one here](https://makersuite.google.com/app/apikey))

### Installation

1. Clone the repository:
```bash
git clone https://github.com/faytranevozter/ai-chat.git
cd ai-chat
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the root directory and add your Gemini API key:
```bash
cp .env.example .env
```

4. Edit `.env` and replace `your_gemini_api_key_here` with your actual API key:
```
VITE_GEMINI_API_KEY=your_actual_api_key_here
```

### Development

Run the development server:
```bash
npm run dev
```

The application will be available at `http://localhost:5173`

### Build

Build for production:
```bash
npm run build
```

Preview the production build:
```bash
npm run preview
```

## Usage

1. Open the application in your browser
2. Type your message in the input field at the bottom
3. Press Enter or click the send button
4. Wait for the AI to respond

## Technologies Used

- [Vue.js 3](https://vuejs.org/) - Progressive JavaScript framework
- [Vite](https://vitejs.dev/) - Next generation frontend tooling
- [Google Generative AI](https://ai.google.dev/) - Gemini API for AI responses

## License

MIT
