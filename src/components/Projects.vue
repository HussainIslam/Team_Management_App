<template>
    <div>
        <div v-if="status.errored">
            <p>Project is errored</p>
        </div>
        <div v-else>
            <div v-if="status.loading">
                <p>The projects are loading</p>
            </div>
            <div v-else>
                <multiselect 
                v-model="value" 
                :options="options"
                :id="value._id"
                :key="value._id"
                :multiple="true"
                :close-on-select="false"
                :clear-on-select="false"
                :internalSearch="true"
                :preserveSearch="true"
                @close="changeProject"
                @remove="removeProject"
                label="ProjectName"
                trackBy="_id"
                selectLabel="Sel"
                deselectLabel="Rem"
                placeholder="Select Projects"
                >
                </multiselect>
            </div>
        </div>
    </div>
</template>

<script>
import Multiselect from 'vue-multiselect'

export default {
    name: 'Projects',
    props: [ 'teamProjects', 'allProjects' ],
    components:{
        Multiselect
    },
    data: function(){
        return {
            status:{
                loading: false,
                errored: false,
            },
            value: null,
            options: null,
        }
    },
    
    created: function(){
        this.status.loading = true;
        this.options = this.allProjects;
        this.value = this.teamProjects.map(item=>this.allProjects.find(project=>project._id==item));
        this.status.loading = false;
    },
    methods: {
        changeProject: function(item){
            this.$emit('click',this.value);
        },
        removeProject: function(item){
            this.value = this.value.filter(project=>project._id != item._id);
            this.changeProject();
        }
    }

}
</script>

<style scoped>

</style>
