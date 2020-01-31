<template>
    <b-nav>
        <b-nav-item :disabled="true">
        {{numberOfCorrectAnswers}}/ {{ numTotal }} Correct
        </b-nav-item>
        <b-nav-item ml-auto 
        :disabled="true" 
        >Question {{ index + 1 }} of 10</b-nav-item>

    </b-nav>
</template>

<script>
export default {
    props: {
        numberOfCorrectAnswers: Number,
        numTotal: Number,
        index: Number
    }
}
</script>
