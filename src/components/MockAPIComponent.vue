<template>
  <!-- <p>mockapi</p> -->
</template>

<script>
export default {
  name: 'MockApiComponent',
  
  data() {
    return {
      queriesAndResponses: [
        {
          query: 'give me good math problems for 2nd graders',
          response: [
            {
              url: 'https://achievethecore.org/content/upload/2.MD.C.8_NWEA.pdf',
              description: 'This document provides a math problem designed for second graders that involves calculating the total amount of money from various coins, aligning with the 2nd-grade Measurement and Data domain.',
              exploreUrl: 'https://achievethecore.org/content/upload/2.MD.C.8_NWEA.pdf'
            },
            {
              url: 'https://achievethecore.org/content/upload/Gr%202.P.2%20Two%20snakes_Final%20.pdf',
              description: 'This assessment task involves solving a subtraction problem about comparing lengths of two snakes, using visual aids like diagrams and emphasizing fluency with calculations.',
              exploreUrl: 'https://achievethecore.org/content/upload/Gr%202.P.2%20Two%20snakes_Final%20.pdf'
            },
            {
              url: 'https://www.insidemathematics.org/sites/default/files/materials/bikes%20and%20trikes.pdf',
              description: 'This example involves counting cycles and calculating wheels using addition and multiplication, making it suitable for 2nd graders to build number sense and logical reasoning.',
              exploreUrl: 'https://achievethecore.org/content/upload/bikes%20and%20trikes.pdf'
            }
          ]
        },
        {
          query: 'practice for multiplying numbers',
          response: [
            {
              url: 'https://example.com/multiplication.pdf',
              description: 'Multiplication practice sheet for 3rd graders.',
              exploreUrl: 'https://example.com/multiplication-explore.pdf'
            }
          ]
        }
      ]
    };
  },

  created() {
    console.log('Available queries in MockApiComponent:', this.queriesAndResponses);
  },

  methods: {
    findResponse(searchQuery) {
      if (!searchQuery) return null;

      console.log(`User search query: "${searchQuery}"`);

      let bestMatch = null;
      let highestScore = 0;

      this.queriesAndResponses.forEach(item => {
        const score = this.calculateSimilarity(searchQuery, item.query);
        console.log(`Comparing with: "${item.query}" - Similarity Score: ${score}`);

        if (score > highestScore) {
          highestScore = score;
          bestMatch = item.response;
        }
      });

      console.log('Best match response:', bestMatch);

      return bestMatch || null;
    },

    calculateSimilarity(userQuery, predefinedQuery) {
      const userWords = userQuery.toLowerCase().split(' ');
      const queryWords = predefinedQuery.toLowerCase().split(' ');

      const matchingWords = userWords.filter(word => queryWords.includes(word));
      return matchingWords.length / queryWords.length;
    }
  }
};
</script>