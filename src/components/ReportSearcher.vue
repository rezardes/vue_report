<template>
    <div>
      <v-text-field
        append-icon="search"
        v-model="search"
        color="teal"
        label="Enter Keywords Here"
        single-line
        hide-details
      ></v-text-field>
      <v-btn fab dark color="teal">
        <v-icon dark>add</v-icon>
      </v-btn>
    </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
