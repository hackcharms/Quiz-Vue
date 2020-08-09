<template>
    <div>
        <b-nav>
            <b-nav-item disabled>Quiz Bar</b-nav-item>
            <b-nav-item disabled>Counter {{index+1}}/{{totalQuestions}}</b-nav-item>
            <b-nav-item disabled>Score {{score}}/{{totalQuestions}}</b-nav-item>
        </b-nav>
    </div>
</template>
<script>
export default {
    props:{
        index:Number,
        totalQuestions:Number,
        score:Number,
        
    }
}
</script>