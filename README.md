# chandrachoodR.github.io
```html
<html>
<head>
  <title>ChatGPT UI</title>
  <link rel="stylesheet" href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css">
  <style>
    .chat-container {
      height: 80vh;
      overflow-y: auto;
    }
    .chat-message {
      max-width: 60%;
      word-break: break-word;
    }
    .error-message {
      color: red;
    }
  </style>
</head>
<body class="bg-gray-900 text-white">
  <div class="container mx-auto p-4">
    <h1 class="text-4xl font-bold">ChatGPT UI</h1>
    <p class="text-lg">A simple chat interface for OpenAI's chat models.</p>
    <div class="chat-container mt-4 border border-gray-700 rounded-lg p-2">
      <!-- Chat messages will be appended here -->
    </div>
    <form id="chat-form" class="mt-4 flex">
      <input id="chat-input" type="text" class="flex-1 bg-gray-800 border border-gray-700 rounded-lg p-2 text-gray-300 focus:outline-none focus:border-blue-500" placeholder="Type your message here...">
      <button id="chat-submit" type="submit" class="ml-2 bg-blue-500 hover:bg-blue-600 rounded-lg p-2 text-white font-bold">Send</button>
    </form>
  </div>
  <script>
    // Get the elements from the document
    const chatForm = document.getElementById("chat-form");
    const chatInput = document.getElementById("chat-input");
    const chatContainer = document.getElementById("chat-container");
    const chatSubmit = document.getElementById("chat-submit");

    // Define some constants for the API
    const API_URL = "https://api.openai.com/v1/engines/gpt-3.5/completions";
    const API_KEY = "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"; // Replace with your own API key
    const MODEL = "text-davinci-003"; // The model to use
    const ENGINE = "davinci"; // The engine to use
    const TEMPERATURE = 0.9; // The temperature parameter for the model
    const MAX_TOKENS = 150; // The maximum number of tokens to generate
    const FREQUENCY_PENALTY = 0; // The frequency penalty parameter for the model
    const PRESENCE_PENALTY = 0.6; // The presence penalty parameter for the model

    // Define a function to create a chat message element
    function createChatMessage(text, isUser) {
      // Create a div element for the message
      const messageDiv = document.createElement("div");
      // Add the chat-message class and a conditional class for user or bot
      messageDiv.classList.add("chat-message", isUser ? "bg-blue-500 text-white self-end rounded-tl-lg rounded-bl-lg rounded-tr-lg p-2 m-2" : "bg-gray-800 text-gray-300 self-start rounded-tl-lg rounded-br-lg rounded-tr-lg p-2 m-2");
      // Set the inner text to the message text
      messageDiv.innerText = text;
      // Return the message element
      return messageDiv;
    }

    // Define a function to create an error message element
    function createErrorMessage(text) {
      // Create a div element for the error message
      const errorDiv = document.createElement("div");
      // Add the error-message class and some tailwind classes for styling
      errorDiv.classList.add("error-message", "self-start bg-red-500 text-white rounded-lg p-2 m-2");
      // Set the inner text to the error text
      errorDiv.innerText = text;
      // Return the error element
