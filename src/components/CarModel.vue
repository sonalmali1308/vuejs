
<script setup lang="ts">
import {ref,reactive,onMounted} from 'vue';
import axios from 'axios';

const model_data=ref([{
        code: "S",
        description: "Model S",
        colors: [
          { code: "white", description: "Pearl White Multi-Coat", price: 0,'img':'https://interstate21.com/tesla-app/images/S/white.jpg' },
          { code: "black", description: "Solid Black", price: 0,'img':'https://interstate21.com/tesla-app/images/S/black.jpg' },
          { code: "blue", description: "Deep Blue Metallic", price: 0,'img':'https://interstate21.com/tesla-app/images/S/blue.jpg' },
          { code: "grey", description: "Stealth Grey", price: 0,'img':'https://interstate21.com/tesla-app/images/S/grey.jpg' },
          { code: "red", description: "Ultra Red", price: 0,'img':'https://interstate21.com/tesla-app/images/S/red.jpg' }
        ]
      },
      {
        code: "X",
        description: "Model X",
        colors: [
          { code: "white", description: "Pearl White Multi-Coat", price: 0,'img':'https://interstate21.com/tesla-app/images/X/white.jpg' },
          { code: "black", description: "Solid Black", price: 0,'img':'https://interstate21.com/tesla-app/images/X/black.jpg' },
          { code: "blue", description: "Deep Blue Metallic", price: 0,'img':'https://interstate21.com/tesla-app/images/X/blue.jpg' },
          { code: "grey", description: "Stealth Grey", price: 0,'img':'https://interstate21.com/tesla-app/images/X/grey.jpg' },
          { code: "red", description: "Ultra Red", price: 0,'img':'https://interstate21.com/tesla-app/images/X/red.jpg' }
        ]
      },
      {
        code: "C",
        description: "Cybertruck",
        colors: [
          { code: "grey", description: "Stainless Steel", price: 0,'img':'https://interstate21.com/tesla-app/images/C/grey.jpg' },
          { code: "black", description: "Satin Black", price: 6500,'img':'https://interstate21.com/tesla-app/images/C/black.jpg' },
          { code: "white", description: "Satin White", price: 6500,'img':'https://interstate21.com/tesla-app/images/C/white.jpg' }
        ]
      },
      {
        code: "3",
        description: "Model 3",
        colors: [
          { code: "white", description: "Pearl White Multi-Coat", price: 1000,'img':'https://interstate21.com/tesla-app/images/3/white.jpg' },
          { code: "black", description: "Solid Black", price: 1500,'img':'https://interstate21.com/tesla-app/images/3/black.jpg' },
          { code: "blue", description: "Deep Blue Metallic", price: 1000,'img':'https://interstate21.com/tesla-app/images/3/blue.jpg' },
          { code: "grey", description: "Midnight Silver Metallic", price: 0,'img':'https://interstate21.com/tesla-app/images/3/grey.jpg' },
          { code: "red", description: "Red Multi-Coat", price: 2000,'img':'https://interstate21.com/tesla-app/images/3/red.jpg' }
        ]
      },
      {
        code: "Y",
        description: "Model Y",
        colors: [
          { code: "white", description: "Pearl White Multi-Coat", price: 1000,'img':'https://interstate21.com/tesla-app/images/Y/white.jpg' },
          { code: "black", description: "Solid Black", price: 2000,'img':'https://interstate21.com/tesla-app/images/Y/black.jpg' },
          { code: "blue", description: "Deep Blue Metallic", price: 1000,'img':'https://interstate21.com/tesla-app/images/Y/blue.jpg' },
          { code: "grey", description: "Midnight Silver Metallic", price: 0,'img':'https://interstate21.com/tesla-app/images/Y/grey.jpg' },
          { code: "red", description: "Red Multi-Coat", price: 2000,'img':'https://interstate21.com/tesla-app/images/Y/red.jpg' }
        ]
      }]);


const options_model_data = ref([{
    "S": {
    configs: [
        { id: 1, description: "Dual Motor All-Wheel Drive", range: 405, speed: 149, price: 74990 },
        { id: 2, description: "Plaid - Tri Motor All-Wheel Drive", range: 396, speed: 200, price: 89990 },
    ],
    towHitch: false,
    yoke: true
    },
    "X": {
    configs: [
        { id: 1, description: "Dual Motor All-Wheel Drive", range: 348, speed: 149, price: 79990 },
        { id: 2, description: "Plaid - Tri Motor All-Wheel Drive", range: 333, speed: 149, price: 94990 },
    ],
    towHitch: true, // costs $1,000
    yoke: true, // costs $1,000
    },
    "C": {
    configs: [
        { id: 1, description: "Rear Wheel Drive", range: 250, speed: 110, price: 60990 },
        { id: 2, description: "Dual Motor All-Wheel Drive", range: 340, speed: 112, price: 79990 },
        { id: 3, description: "Cyberbeast - Tri Motor All-Wheel Drive", range: 320, speed: 130, price: 99990 },
    ],
    towHitch: true, // costs $1,000
    yoke: true, // costs $1,000
    },
    "3": {
    configs: [
        { id: 1, description: "Rear-Wheel Drive", range: 272, speed: 140, price: 38990 },
        { id: 2, description: "Long Range - Dual Motor All-Wheel Drive", range: 333, speed: 145, price: 45990 },
        { id: 3, description: "Performance - Dual Motor All-Wheel Drive", range: 315, speed: 162, price: 50990 },
    ],
    towHitch: false,
    yoke: false,
    },
    "Y": {
    configs: [
        { id: 1, description: "Rear-Wheel Drive", range: 260, speed: 135, price: 43990 },
        { id: 2, description: "Long Range - Dual Motor All-Wheel Drive", range: 330, speed: 135, price: 48990 },
        { id: 3, description: "Performance - Dual Motor All-Wheel Drive", range: 303, speed: 155, price: 52490 },
    ],
    towHitch: true,
    yoke: false,
    }
}]);


const filtercolor=ref([]);
const filterImage=ref([]);
const modelSelect=ref();
const colorSelect=ref();
const configSelect=ref();
const option_model_filter_data=ref();
const step2=ref(false);
const range=ref(false);
const speed=ref(false);
const price=ref(false);
const config_description=ref('');
const config_price=ref([]);
const step2disable=ref(true);
const step3disable=ref(true);
function getcolors(code_model){
    filtercolor.value=model_data.value.find((val)=>val.code==code_model);
    return filtercolor;
}

function getColorImage(color_code){
    filtercolor.value=model_data.value.find((val)=>val.code==modelSelect.value);
    filterImage.value=filtercolor.value.colors.find((val)=>val.code==color_code);
}
const step1_class=ref('');
const step2_class=ref('d-none');
const step3_class=ref('d-none');

function step_click(step=''){

    if(step=='step1'){
        step1_class.value='';
        step2_class.value='d-none';
        step3_class.value='d-none';
    }
    else if(step=='step2'){
        step1_class.value='d-none';
        step2_class.value='';
        step3_class.value='d-none';
        getModelConfig();
    }
    else{
        step1_class.value='d-none';
        step2_class.value='d-none';
        step3_class.value='';
    }
}

function getModelConfig(){
    if(modelSelect.value!==''){
        option_model_filter_data.value=options_model_data.value[0][modelSelect.value];
        if(option_model_filter_data.value.configs!==undefined){
            step2.value=true;
        }
    }
}

function getconfigitem(config_id){
    option_model_filter_data.value=options_model_data.value[0][modelSelect.value];
    if(config_id!=''){
        const current_config_data=option_model_filter_data.value.configs.find((val)=>val.id==config_id);
        range.value=current_config_data['range'];
        speed.value=current_config_data['speed'];
        price.value=current_config_data['price'];
        config_description.value=current_config_data['description'];
        config_price.value=current_config_data['price'];

    }
}

if(modelSelect.value!=undefined && colorSelect.value!=undefined){
    step2disable.value=false;

    if(configSelect.value!=undefined){
        step3disable.value=false;
    }
}

</script>

<template>
        <div class="d-flex">
            <li href="#step1" class="m-10" @click="step_click('step1')">
                Step1
            </li>
            <li href="#step2" class="m-10"  @click="step_click('step2')" :disabled="step2disable">
                Step2
            </li>
            <li href="#step3" class="m-10" :class="step3disable" @click="step_click('step3')" :disabled="step3disable">
                Step3
            </li>
        </div>
            <div  id="step1"  :class=step1_class>

                    <h1>Step 1:Choose your Model and Color</h1>
                    <select id="modelSelect" v-model="modelSelect" @change="getcolors($event.target.value)">
                        <option  value="" >Select</option>
                        <option  v-for="car_item in model_data" :value=car_item.code>{{ car_item.description }}</option>
                    </select>
                    <select  id="colorSelect" v-model="colorSelect"  @change="getColorImage($event.target.value)">    
                        <option  v-for="color_item in filtercolor.colors" :value=color_item.code>{{ color_item.description }}</option>
                    </select>
                
            </div>
            
            <div id="step2" :class="step2_class">
                <h1>Step 2:Select Your config and options</h1>
                    <div   v-if="step2==1 " class="d-flex">
                        <select id="configSelect" v-model="configSelect" @change="getconfigitem($event.target.value)">
                                <option  value="" >Select</option>
                                <option  v-for="config_item in option_model_filter_data.configs" :value=config_item.id>{{ config_item.description }}</option>
                        </select>
                        <div v-if="configSelect">
                        <span>
                                Range:{{range }}-Max speed:{{speed }} -Cost:{{price }}
                        </span>

                            <label>Tow Witch?</label>
                            <input type="checkbox" id="includeYoke" value="Jack" v-model="towHitch" :checked="option_model_filter_data.towHitch == 1 ? true: false">
                            <label>Yoke streeing wheel?</label>
                            <input type="checkbox" id="includeTow" value="Jack" v-model="yoke" :checked="option_model_filter_data.yoke == 1 ? true: false"> 
                        </div>
                    </div>

            </div>
            <div id="step3" :class="step3_class ">
                    <h1>Step3:Summary</h1>
                    <table>
                     <tr v-if="filtercolor.description">
                        <td >{{ filtercolor.description }}</td>
                    </tr>   
                    <tr v-if="configSelect && config_description">
                        <td>
                            {{ config_description }}
                            {{ '$'+config_price }}
                        </td>
                    </tr>
                    <tr v-if="configSelect">
                        <td>
                            Range:{{range }}-Max speed:{{speed }}
                        </td>
                    </tr>
                    <tr v-if="filterImage.description">
                        <td>
                            {{ filterImage.description }}
                        </td>
                        <td>
                            {{ '$'+filterImage.price }}
                        </td>
                    </tr>
                    <tr v-if="option_model_filter_data && option_model_filter_data.towHitch==1">
                        <td>
                            Tow Hitech Package
                        </td>
                        <td>
                        </td>
                    </tr>
                    <tr >
                        <td>
                            Total Summary
                        </td>
                        <td>
                            {{ '$'+config_price+filterImage.price }}
                        </td>
                    </tr>
                    </table>

            </div>
            <img :src="filterImage.img">
</template>