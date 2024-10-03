<template>
    <div class="chat-component">
      <div class="chat-bar">
        <textarea
          v-model="searchQuery"
          :readonly="isSearching || isLoading"
          @keydown.enter.prevent="handleEnterKey"
          @keydown.shift.enter.exact="insertNewLine"
          placeholder="Type your message here"
          rows="1"
          ref="chatBox"
          @input="onInput"
        ></textarea>
      </div>
  
      <!-- Button container to keep both buttons in the same space -->
      <div class="button-wrapper">
        <!-- Show loading spinner if loading -->
        <div v-if="isLoading" class="no-transition-spinner">
          <CirclesToRhombusesSpinner
            :animation-duration="1200"
            :circles-num="3"
            :circle-size="15"
            color="#A9A9A9"
          />
        </div>
  
        <!-- Search button -->
        <button v-if="!isSearching && !isLoading" @click="handleEnterKey" class="no-transition">Search</button>
  
        <!-- Search Again button -->
        <button v-if="isSearching && !isLoading" @click="handleSearchAgain" class="no-transition">Search Again</button>
      </div>
    </div>
  </template>
  
  <script>
  import { CirclesToRhombusesSpinner } from 'epic-spinners'; // Import the spinner component
  
  // A sample response of what the API might return from searching
  const sample_response = [
    {
      url: 'https://achievethecore.org/content/upload/2.MD.C.8_NWEA.pdf',
      description: 'This document provides a math problem designed for second graders that involves calculating the total amount of money from various coins, aligning with the 2nd-grade Measurement and Data domain.',
      exploreUrl: 'https://achievethecore.org/content/upload/2.MD.C.8_NWEA.pdf'
    },
    {
      url: 'https://achievethecore.org/content/upload/Gr%202.P.2%20Two%20snakes_Final%20.pdf',
      description: 'This assessment task involves solving a subtraction problem about comparing lengths of two snakes, using visual aids like diagrams and emphasizing fluency with calculations.',
      exploreUrl: 'https://achievethecore.org/content/upload/2.MD.C.8_NWEA.pdf'
    },
    {
      url: 'https://www.insidemathematics.org/sites/default/files/materials/bikes%20and%20trikes.pdf',
      description: 'This example involves counting cycles and calculating wheels using addition and multiplication, making it suitable for 2nd graders to build number sense and logical reasoning.',
      exploreUrl: 'https://achievethecore.org/content/upload/2.MD.C.8_NWEA.pdf'
    }
  ];
  
  export default {
    components: {
      CirclesToRhombusesSpinner, // Register the spinner component
    },
    data() {
      return {
        searchQuery: '', // Stores the user's input
        isSearching: false, // Tracks if a search has been triggered
        isLoading: false, // Tracks if the loading spinner is being shown
        hasSearched: false // Tracks if a search was completed before
      };
    },
    methods: {
      // A helper function that returns a promise-based delay
      delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
      },
      async handleEnterKey() {
        if (this.searchQuery.trim() !== '') {
          this.isLoading = true; // Show the loading spinner
          this.hasSearched = true; // Mark that a search has been triggered
  
          // Wait for 5 seconds
          await this.delay(5000);
  
          this.isLoading = false; // Hide the loading spinner
          this.isSearching = true; // Show the "Search Again" button
          console.log(sample_response)
          this.$emit('search', sample_response); // Emit the search result to the parent
        }
      },
      handleSearchAgain() {
        this.resetSearch(); // Reset the search component
        this.$emit('clear'); // Emit an event to the parent to clear the search result
      },
      insertNewLine() {
        const textarea = this.$refs.chatBox;
        const cursorPosition = textarea.selectionStart;
        this.searchQuery =
          this.searchQuery.slice(0, cursorPosition) +
          '\n' +
          this.searchQuery.slice(cursorPosition);
        this.$nextTick(() => {
          textarea.selectionStart = cursorPosition + 1;
          textarea.selectionEnd = cursorPosition + 1;
        });
      },
      onInput() {
        this.autoResize();
      },
      autoResize() {
        const textarea = this.$refs.chatBox;
        textarea.style.height = 'auto'; // Reset height to auto to calculate the correct height
        textarea.style.height = textarea.scrollHeight + 'px'; // Set height to match scrollHeight
      },
      resetSearch() {
        // Reset the component to its initial state
        this.searchQuery = ''; // Clear the input
        this.isSearching = false; // Show the "Search" button
        this.hasSearched = false; // Reset search state
        this.autoResize(); // Ensure proper resizing
      }
    },
    mounted() {
      // Set initial height on mount based on current content
      this.autoResize();
    }
  };
  </script>
  
  <style scoped>
  .chat-component {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 10px;
  }
  
  .chat-bar textarea {
    width: 400px;
    padding: 12px 8px;
    font-size: 16px;
    border: none;
    border-radius: 8px;
    resize: none;
    overflow: hidden;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
    min-height: 40px;
    line-height: 1.5;
    box-sizing: border-box;
  }
  
  .chat-bar textarea:focus {
    outline: none;
  }
  
  /* Wrapper to ensure the buttons occupy the same vertical space */
  .button-wrapper {
    position: relative;
    width: 140px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  button {
    width: 100%;
    padding: 8px 16px;
    font-size: 16px;
    cursor: pointer;
  }
  
  /* No transition on the search and search again buttons */
  .no-transition {
    transition: none !important;
  }
  
  /* Override spinner transition */
  .no-transition-spinner {
    animation: none !important; /* Disable any fade/transition animations */
  }
  </style>
  