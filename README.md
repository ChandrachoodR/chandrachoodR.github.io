
<html>
<head>
  <title>Chatgpt UI</title>
  <link rel="stylesheet" href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css">
  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
</head>
<body class="bg-gray-900 text-white">
  <div class="container mx-auto p-4">
    <h1 class="text-4xl font-bold text-center">Chatgpt UI</h1>
    <div class="flex flex-col mt-8">
      <div id="messages" class="overflow-y-auto h-96 mb-4 border border-gray-700 rounded-lg p-2">
        <!-- Messages will be appended here -->
      </div>
      <form id="form" class="flex">
        <input id="input" type="text" class="flex-1 bg-gray-800 border border-gray-700 rounded-l-lg p-2" placeholder="Type your prompt here...">
        <button id="button" type="submit" class="bg-blue-600 border border-blue-600 rounded-r-lg p-2">Send</button>
      </form>
    </div>
  </div>
  <script>
    // Get the elements from the document
    const form = document.getElementById("form");
    const input = document.getElementById("input");
    const button = document.getElementById("button");
    const messages = document.getElementById("messages");

    // Define the OpenAI parameters
    const endpoint = "https://api.openai.com/v1/completions";
    const model = "text-davinci-003";
    const key = "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"; // Replace with your own key
    const headers = {
      "Authorization": `Bearer ${key}`,
      "Content-Type": "application/json"
    };

    // Define a function to append a message to the messages div
    function appendMessage(text, sender) {
      // Create a new div element
      const message = document.createElement("div");
      // Add a class based on the sender
      message.className = sender === "user" ? "bg-gray-800 text-white rounded-br-lg rounded-tl-lg p-2 my-2 ml-auto w-max" : "bg-blue-600 text-white rounded-bl-lg rounded-tr-lg p-2 my-2 mr-auto w-max";
      // Set the text content to the text argument
      message.textContent = text;
      // Append the message to the messages div
      messages.appendChild(message);
      // Scroll to the bottom of the messages div
      messages.scrollTop = messages.scrollHeight;
    }

    // Define a function to send a request to OpenAI and get a response
    async function getResponse(prompt) {
      // Disable the button and input while waiting for the response
      button.disabled = true;
      input.disabled = true;
      // Create a data object with the prompt and model parameters
      const data = {
        prompt: prompt,
        model: model
      };
      try {
        // Send a post request to the endpoint with the data and headers
        const response = await axios.post(endpoint, data, {headers: headers});
        // Get the text from the response data
        const text = response.data.choices[0].text;
        // Append the text as a message from chatgpt
        appendMessage(text, "chatgpt");
      } catch (error) {
        // If there is an error, append it as a message from chatgpt
        appendMessage(error.message, "chatgpt");
      }
      // Enable the button and input after getting the response
      button.disabled = false;
      input.disabled = false;
    }

    // Add an event listener to the form submit event
    form.addEventListener("submit", (event) => {
      // Prevent the default form submission behavior
      event.preventDefault();
      // Get the value of the input element
      const prompt = input.value;
      // If the prompt is not empty, append it as a message from user and get a response from chatgpt
      if (prompt) {
        appendMessage(prompt, "user");
        getResponse(prompt);
        // Clear the input value after sending it
        input.value = "";
      }
    });
  </script>
</body>
</html>

