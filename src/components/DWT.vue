<template>
    <div id="dwt-container">
    </div>
  </template>
   
  <script>
  import dwt from 'dwt'
   
  export default {
    name: 'dwt',
    data () {
      return {
        dwt: {
          obj: null,
          id: 'dwtObject',
          licenseKey: 't0195AgYAAGOXnlU5sZm0vkFC3DvEcV0bLh+2QASQxjfU7UZNgViou22iHZcRyLFFc6vTyMaRA23iQ9qT8cqeNKrlv+falsAwN9e8m9eVVupotYpWUUXLOlq5qwW6aRjn7++mqf85koHPBOh6QFaAJTCXswP6dimPFiAWoAagVg20gNNVHPdfsI9M+dbYf1w6pYJTn3eWSXnGyQpOueVMAfEpFN+02nEGWJ6cDSAWoKcAl5/aISDYA2IBegA6J9Eh+h/MdDSo'  // Your license key here
        }
      }
    },
    mounted () {
      this.mountDWT()
    },
    methods: {
      mountDWT () {
        return new Promise((res, rej) => {
          this.unmountDWT()
            .then(() => {
              dwt.WebTwainEnv.UseLocalService = true;
              dwt.WebTwainEnv.ResourcesPath = "resources/dwt";
              dwt.WebTwainEnv.ProductKey = this.dwt.licenseKey
              dwt.WebTwainEnv.AutoLoad = false;
              dwt.WebTwainEnv.Containers = [];
              dwt.WebTwainEnv.IfAddMD5InUploadHeader = false;
              dwt.WebTwainEnv.IfConfineMaskWithinTheViewer = false;
              let dwtConfig = { WebTwainId: this.dwt.id }
              // By default, use local service is true
              dwt.WebTwainEnv.CreateDWTObjectEx(
                    dwtConfig, 
                    (dwtObject) => { this.dwt.obj = dwtObject; res(true);},
                    (errStr) => { console.log(`failed to initialize dwt, message: ${errStr}`); rej(false);}
              )
            })
        })
      },
      /**
       * Delete dwt instance
       */
      unmountDWT () {
        return new Promise((res, rej) => {
          if (dwt.WebTwainEnv.DeleteDWTObject(this.dwt.id)) {
            res(true)
          } else {
            rej(false)
          }
        })
      }
    }
  }
  </script>
   
  <style scoped>
   
  </style> 