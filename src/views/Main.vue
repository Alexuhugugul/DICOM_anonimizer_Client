<template>
    <div>
        Anonimize File
        <form id="fileForm" enctype="multipart/form-data">
            <input type="file" name="fi" id="fileInput" @change="fileChanged">
            <div v-if="linksShowed">
                <a v-for="link in links" :key="link" :href="link" style="display: block;">Download Anon File</a>
            </div>
        </form>
        <hr>
        Deanonimize File
        <form id="DfileForm" enctype="multipart/form-data">
            <input type="file" name="dfi" id="fileInput" @change="DfileChanged">
            <div v-if="DlinkShowed">
                <a v-for="link in Dlinks" :key="link" :href="link" style="display: block;">Download Deanon File</a>
            </div>
        </form>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data () {
        return {
            linksShowed: false,
            DlinkShowed: false,
            links: null,
            Dlinks: null
        }
    },
    methods: {
        async fileChanged ( event ) {
            const formData = new FormData( fileForm )

            axios.post( 'http://localhost:8082/anonimize', formData ).then( ( res ) => {
                this.links = res.data.links.map( link => `http://localhost:8082/${ link }` )
                this.linksShowed = true
            })
        },
        DfileChanged ( event ) {
            const formData = new FormData( DfileForm )

            axios.post( 'http://localhost:8082/deanonimize', formData ).then( ( res ) => {
                this.Dlinks = res.data.links.map( link => `http://localhost:8082/${ link }` )
                this.DlinkShowed = true
            })
        }
    }
}
</script>

<style lang="scss" scoped>

</style>
