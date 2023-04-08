<template>
    <!-- start of showing project in the table -->
    <div class="overflow-auto">
       <table class="shadow-2xl border-2 border-gray-800 w-full">
        <thead class="bg-gray-800 text-white text-left">
            <tr>
                <th class="py-3">Name</th>
                <th class="py-3">listing Type</th>
                <th class="py-3">category</th>
                <th class="py-3">subcategory</th>
                <th class="py-3">status</th>
                <th class="py-3">details</th>
                <th class="py-3">specifications</th>
                <th class="py-3">Ageproject</th>
                <th class="py-3">logurl</th>
                <th class="py-3">defaulturl</th>
                <th class="py-3">approvestatus</th>
                <th class="py-3">Edit Action</th>
                <th class="py-3">Delete Action</th>
            </tr>
        </thead>
        <tbody class="divide-y  divide-gray-800">
            <tr v-for="(project,index) in ProjectUrlData" :key="project" class="text-left cursor-pointer">
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.name" class="focus:border-purple-500" cols="10" rows="1">{{project.name}}</textarea>
                <p v-else>{{project.name}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.listing_type_name" class="focus:border-purple-500" cols="10" rows="1">{{project.listing_type_name}}</textarea>
                <p v-else>{{project.listing_type_name}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.category" class="focus:border-purple-500" cols="10" rows="1">{{project.category}}</textarea>
                <p v-else>{{project.category}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.status" class="focus:border-purple-500" cols="10" rows="1">{{project.status}}</textarea>
                <p v-else>{{project.status}}</p>
              </td> 
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.details" class="focus:border-purple-500" cols="10" rows="1">{{project.details}}</textarea>
               <p v-else>{{project.details}}</p>
             </td>          
             <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.specifications" class="focus:border-purple-500" cols="10" rows="1">{{project.specifications}}</textarea>
               <p v-else>{{project.specifications}}</p>
             </td>  
             <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.age_of_the_project" class="focus:border-purple-500" cols="10" rows="1">{{project.age_of_the_project}}</textarea>
               <p v-else>{{project.age_of_the_project}}</p>
             </td>  
             <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editProjectIndex===index" v-model="editProjectForm.logo_url" class="focus:border-purple-500" cols="10" rows="1">{{project.logo_url}}</textarea>
               <p v-else>{{project.logo_url}}</p>
             </td>  
             <td class="py-3 pr-6 whitespace-nowrap">
              <textarea v-if="editProjectIndex===index" v-model="editProjectForm.default_image_url" class="focus:border-purple-500" cols="10" rows="1">{{project.default_image_url}}</textarea>
              <p v-else>{{project.default_image_url}}</p>
            </td> 
            <td class="py-3 pr-6 whitespace-nowrap">
              <textarea v-if="editProjectIndex===index" v-model="editProjectForm.approve_status" class="focus:border-purple-500" cols="10" rows="1">{{project.approve_status}}</textarea>
              <p v-else>{{project.approve_status}}</p>
            </td> 
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionProjectEdit @edit="editproject(project,index)"/>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionProjectDelete @delete="deleteproject(project)"/>
              </td>
            </tr>
        </tbody>
       </table>
          
    </div>
     <!-- End of showing project in the table -->
</template>
<script setup lang="ts">
//Defining the schema of projectschema
interface ProjectSchema{
 name:string,
 listing_type_name:string,
 category:string,
 status:string,
 details:string,
 specifications:string
 age_of_the_project:string,
 logo_url:string,
 default_image_url:string,
 approve_status:string
}
// Getting the props of projecturlData
let props = defineProps<{ ProjectUrlData:ProjectSchema }>()
const emits = defineEmits(['edit','delete']);
const editProjectIndex=ref("0")


//Defining the schema of reactive ProjectEditForm
interface ProjectEditForm {
  name:string,
 listing_type_name:string,
 category:string,
 status:string,
 details:string,
 specifications:string
 age_of_the_project:string,
 logo_url:string,
 default_image_url:string,
 approve_status:string
 
}

//Declaring in the empty form and use it in v-model
const editProjectForm:ProjectEditForm=reactive({
  name:'',
 listing_type_name:'',
 category:'',
 status:'',
 details:'',
 specifications:'',
 age_of_the_project:'',
 logo_url:'',
 default_image_url:'',
 approve_status:''
})

//Emitting the delete and pass it in the main
const deleteproject=(project:any)=>{
   emits('delete',project)
 
}
//Emitting the edit and pass it in the main
const editproject=(project:any,index:any)=>{
   editProjectIndex.value=index
 emits('edit',project,editProjectForm)
}
</script>