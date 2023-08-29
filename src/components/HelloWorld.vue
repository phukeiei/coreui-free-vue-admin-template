<template>
    <div class="hello">
      <h1></h1>
      <select id="sources" v-model="selectedIndex">
        <option
          v-for="(source, index) in sourceList"
          v-bind:value="index"
          :key="index"
        >
          {{ source.displayName }}
        </option>
      </select>
      <button @click="acquireImage()">Acquire Images</button>
      <button @click="openImage()">Open Documents</button>
      <br />
      <br />
      <div v-bind:id="containerId"></div>
    </div>
  </template>
  <script>
  import Dynamsoft from "dwt";
  export default {
    name: "HelloWorld",
    props: {
      msg: String,
    },
    data() {
      return {
        containerId: "dwtControlContainer",
        sourceList: [],
        selectedIndex: 0,
      };
    },
    mounted() {
      /**
       * ResourcesPath & ProductKey must be set in order to use the library!
       */
      Dynamsoft.DWT.ResourcesPath = "/dwt-resources";
      Dynamsoft.DWT.ProductKey = "t0195AgYAAGOXnlU5sZm0vkFC3DvEcV0bLh+2QASQxjfU7UZNgViou22iHZcRyLFFc6vTyMaRA23iQ9qT8cqeNKrlv+falsAwN9e8m9eVVupotYpWUUXLOlq5qwW6aRjn7++mqf85koHPBOh6QFaAJTCXswP6dimPFiAWoAagVg20gNNVHPdfsI9M+dbYf1w6pYJTn3eWSXnGyQpOueVMAfEpFN+02nEGWJ6cDSAWoKcAl5/aISDYA2IBegA6J9Eh+h/MdDSo";
      Dynamsoft.DWT.Containers = [
        {
          WebTwainId: "dwtObject",
          ContainerId: this.containerId,
          Width: "100%",
          Height: "400px",
        },
      ];
      Dynamsoft.DWT.RegisterEvent("OnWebTwainReady", () => {
        this.Dynamsoft_OnReady();
      });
      Dynamsoft.DWT.Load();
    },
    methods: {
      /**
       * Dynamsoft_OnReady is called when a WebTwain instance is ready to use.
       * In this callback we do some initialization.
       */
      Dynamsoft_OnReady() {
        this.DWObject = Dynamsoft.DWT.GetWebTwain(this.containerId);
        this.DWObject.GetDevicesAsync()
          .then((deviceList) => {
            this.sourceList = deviceList;
          })
          .catch((e) => {
            console.error(e);
          });
      },
      /**
       * Acquire images from scanners or cameras or local files
       */
      acquireImage() {
        if (!this.DWObject) {
          alert("dwt init fail");
          return;
        }
        if (this.DWObject.UseLocalService) {
          this.DWObject.SelectDeviceAsync(this.sourceList[this.selectedIndex])
            .then(() => {
              return this.DWObject.OpenSourceAsync();
            })
            .then(() => {
              return this.DWObject.AcquireImageAsync({});
            })
            .catch((e) => {
              console.error(e);
            })
            .finally(() => {
              console.log("CloseSource")
              this.DWObject.CloseSourceAsync().catch((e) => {
                console.error(e);
              });
            });
        }
      },
      /**
       * Open local images.
       */
      openImage() {
        if (!this.DWObject) {
          alert("dwt init fail");
          return;
        }
        this.DWObject.IfShowFileDialog = true;
        /**
         * Note:
         * This following line of code uses the PDF Rasterizer which is an extra add-on that is licensed seperately
         */
        this.DWObject.Addon.PDF.SetConvertMode(
          Dynamsoft.DWT.EnumDWT_ConvertMode.CM_RENDERALL
        );
        this.DWObject.LoadImageEx(
          "",
          Dynamsoft.DWT.EnumDWT_ImageType.IT_ALL,
          () => {
            //success
          },
          () => {
            //failure
          }
        );
      },
    },
  };
  </script>