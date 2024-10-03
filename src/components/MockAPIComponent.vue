<script>
export default {
  name: 'MockApiComponent',
  
  data() {
    return {
      // Hardcoded queries and responses
      queriesAndResponses: [
        {
          query: 'find second grade math problems',
          response: {
            description: 'A set of second-grade math problems focusing on basic arithmetic.',
            pdfUrl: 'https://example.com/grade2-math.pdf'
          }
        },
        {
          query: 'find subtraction practice for first graders',
          response: {
            description: 'A set of subtraction practice worksheets for first graders.',
            pdfUrl: 'https://example.com/first-grade-subtraction.pdf'
          }
        },
        {
          query: 'practice for multiplying numbers',
          response: {
            description: 'Practice material for multiplying numbers, suitable for third graders.',
            pdfUrl: 'https://example.com/multiplication-practice.pdf'
          }
        }
        // Add more queries and responses here
      ]
    };
  },

  methods: {
    /**
     * Finds the most similar hardcoded query and returns its response.
     * Currently, it uses simple string comparison but could be extended to more sophisticated methods.
     * @param {string} searchQuery - The query entered by the user.
     * @returns {object|null} - Returns the matching response object or null if no match is found.
     */
    findResponse(searchQuery) {
      // If no queries, return null
      if (!searchQuery) return null;
      
      // Use a simple matching technique (you could use Levenshtein or something more advanced)
      const mostSimilar = this.queriesAndResponses.reduce((bestMatch, item) => {
        const similarityScore = this.calculateSimilarity(searchQuery, item.query);
        if (similarityScore > bestMatch.score) {
          return { item, score: similarityScore };
        }
        return bestMatch;
      }, { item: null, score: 0 });

      // Return the response of the most similar query
      return mostSimilar.item ? mostSimilar.item.response : null;
    },

    /**
     * Simple similarity calculation based on common substrings or string matching.
     * For now, we'll use a basic match but you can extend this with more advanced techniques.
     * @param {string} str1 - First string to compare.
     * @param {string} str2 - Second string to compare.
     * @returns {number} - A score of similarity (higher is better).
     */
    calculateSimilarity(str1, str2) {
      const searchWords = str1.toLowerCase().split(' ');
      const queryWords = str2.toLowerCase().split(' ');

      // Count how many words match between the two queries
      const matchingWords = searchWords.filter(word => queryWords.includes(word));

      // Return the match score (simple word overlap count)
      return matchingWords.length / queryWords.length;
    }
  }
};
</script>