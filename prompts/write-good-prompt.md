# WRITING GOOD PROMPTS

The quality of what you get from AI tools is directly tied to the quality of your prompts.

## Example 1: React Todo App

### ❌ Bad Prompt
> Build me a todo app in React

**Why It's Bad:**
- Too vague - what features do you want?
- No technical requirements specified
- No UI/UX direction
- The AI has to guess everything

### ✅ Good Prompt
> Build a todo app in React with the following requirements:
> 
> **FEATURES:**
> - Add, edit, delete, and mark todos complete
> - Filter by: All, Active, Completed
> - LocalStorage persistence
> - Drag and drop to reorder
> 
> **TECHNICAL:**
> - React with functional components and hooks
> - Tailwind CSS for styling
> - Responsive design
> 
> **UI:**
> - Clear, minimal interface similar to Todoist
> - Use checkboxes for completion
> - Inline editing for task names

**Why It's Good:**
- Specific features listed
- Technical stack defined
- UI expectations set
- Gives design reference point

---

## Example 2: API Integration

### ❌ Bad Prompt
> Add a weather API to my app

**Why It's Bad:**
- Which weather API?
- What data should it show?
- Where in the app does it go?
- No error handling mentioned

### ✅ Good Prompt
> Integrate the OpenWeatherMap API into my React app:
> 
> **REQUIREMENTS:**
> - Create a WeatherService.js file for API calls
> - Fetch current weather and 5-day forecast
> - Include proper error handling for failed requests
> - Add loading states
> - Display temperature, conditions, and weather icon
> 
> **API DETAILS:**
> - Use OpenWeatherMap API (I have a key)
> - Endpoints needed: current weather and forecast
> - Handle both successful and error responses
> 
> Show me the service file first, then we'll integrate it into components.

**Why It's Good:**
- Specifies which API
- Lists exact data needed
- Mentions error handling and loading states
- Asks for modular code (service file)
- Breaks it into steps
