<template>
    <div class="container">
        <btnFacebook></btnFacebook>
        <div class="columns">
            <div class="column">
                <div class="message" style="max-width:500px" v-for="status in statuses" :key="status.id">
                    <div class="message-header">
                       <p>{{ status.user.name }} said...</p>
                       <p>{{ status.created_at | ago }}</p>
                    </div>
                    <div class="panel-body">
                        {{ status.body }}
                    </div>
                </div>
                <add-to-stream @completed="addStatus"></add-to-stream>
            </div>
        </div>
        <menu-list :items="['one', 'two', 'three']">
            <template slot-scope="props">
                <h5 v-text="props.item"></h5>
            </template> 
        </menu-list>
    </div>
</template>
<script>
// Vue.component('exampleComponent', require('./components/ExampleComponent.vue'));
import btnFacebook from '../components/btnFacebook';
import AddtoStream from '../components/AddtoStream';
import Example from '../components/Example';
import moment from 'moment';
import Status from '../models/Status';

Vue.component('menu-list', Example);
Vue.component('add-to-stream', AddtoStream);
Vue.component('btnFacebook', btnFacebook);
export default {
    components: { AddtoStream, btnFacebook, Example },
    data(){
        return{
            statuses: []
        }
    },
    created(){
        Status.all(statuses => this.statuses = statuses);
    },
    filters: {
        ago(date) {
            return moment(date).fromNow();
        }
    },
    methods: {
        postedOn(status) {
            return moment(status.created_at).fromNow();
        },
        addStatus(status)
        {
            this.statuses.unshift(status);
            alert('Your status has been added to the stream.');
        }
    },
}
</script>