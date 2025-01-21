
# Information Retrieval Agent 

```
def get_stock_ticker_from_model(self, prompt: str) -> Optional[str]:
        """Get stock ticker symbol using a HuggingFace model if regex fails."""
        messages = [
            {
                "role": "user",
                "content": (
                    f"user prompt: '{prompt}'; "
                    "Provide ONLY the STOCK SYMBOL of the company mentioned in the prompt. "
                    "Return just the stock symbol (e.g., 'META'), with no additional text, explanations, or context. "
                    "Do not include any other characters, spaces, punctuation, or words. "
                    "If no stock symbol is found, return nothing at all."
                ),
            }
        ]

        client = InferenceClient(api_key=settings.HUGGINGFACE_API_KEY)

        # Call the model
        response_content = ""
        try:
            # meta-llama/Llama-3.2-1B ,  I tried with the meta-llama 3.2 models , but this model failed to recognize the stock symbol so i stick to mistralai 
            # probably could be the reponsne is not same structure as the mistralai and it could be done with the change of some code 
            stream = client.chat.completions.create(
                model="mistralai/Mistral-7B-Instruct-v0.2",
                messages=messages,
                temperature=0.0,  # Ensure deterministic behavior
                max_tokens=5,  # Restrict to a short length for symbol-only output
                top_p=0.7,
                stream=True,
            )
            output = ""
            # Process streaming chunks
            for chunk in stream:
                if chunk.get("choices"):
                    delta_content = chunk["choices"][0].get("delta", {}).get("content", "")
                    output += delta_content
                    print(delta_content, end="")  # Print as it streams

        except Exception as e:
            print(f"Error calling the model: {e}")
            return None

        # Clean the output: Remove unwanted characters and convert to uppercase
        cleaned_output = re.sub(r"[^A-Z0-9]", "", output.upper())

        return cleaned_output.strip() if cleaned_output else None
```

The method uses the selected model to extract the stock ticker symbol of a company mentioned in the user's prompt. The output is prompt engineered to ensure that only the stock symbol is returned, with no extra context, text, or characters. If no stock symbol is found, it returns None.

### Input Parameters:

prompt: str: A user-provided text which will be coming from user prompted text from frontend that might mention a company for which the stock ticker symbol needs to be retrieved.

### Message Construction:

The method creates a message to pass to the language model. This message contains specific instructions to the model, requesting it to extract only the stock symbol (e.g., 'META') from the given prompt. The message format is strict: it instructs the model to return nothing if no symbol is found and avoid any additional text or explanation.

### Model Call via Hugging Face API

The method uses an InferenceClient to interact with the Hugging Face API. The API call is made using the model Mistral-7B-Instruct-v0.2, which is fine-tuned for instruction-based tasks.

Parameters for the API Call:
- temperature=0.0: Ensures deterministic output (i.e., the same result each time for the same input).
- max_tokens=5: Limits the response to a very short output, since stock symbols are typically short.
- top_p=0.7: This controls the diversity of the output, although the deterministic temperature setting likely overrides much of this.
- stream=True: Streams the output from the model, allowing partial results to be processed and printed in real-time.
Streaming Output Processing:

- The stream=True option means the output is processed in chunks, rather than waiting for the entire response. As the model produces each chunk, the method appends the content to the output variable, and each chunk is printed as it arrives.
Error Handling:

### Return Data

If there’s an issue with the model call (e.g., network issues, invalid API key, or other errors), the method catches the exception and logs the error message. In such a case, the method returns None.
Output Cleaning:

The raw output from the model is cleaned using a regular expression (re.sub). This regex removes any unwanted characters (non-alphanumeric) and ensures the result contains only uppercase letters and numbers, which are typical for stock ticker symbols.

The final output is stripped of any leading or trailing whitespace, and if the result is empty, it returns None.