<template>
    <div>
        <!-- start of project -->
       <div class="p-[50px] flex divide-y-2 border-y">
           
        <h1>Projects</h1>
        <!-- start of add button -->
        <div class="ml-[56rem]">
            <button class="flex bg-slate-100" @click="openProject">
            <PlusIcon class="h-6 w-6"/>
            <p class="ml-3">Add Project</p>
            </button>
        </div>
          <!-- end of add button --> 
            <!-- opening the Project modal by clicking add Project -->
        <CollectionProjectAdd v-show="isProject" @cancel="isProject=false" @save="saveProject"/> 
       </div>
    <div class="text-center">
        <h1 class="text-3xl">Lists of all projects</h1>
         <div>
            <CollectionProjectList  :ProjectUrlData="ProjectUrlData" @delete="deleteProjectListener" @edit="editProjectListener"/>
        </div>
      </div>
    </div> 
     <!-- End of project -->
</template>
<script setup lang="ts">
//importing the HeroIcon and  useAuthLazyFetch
import { PlusIcon } from "@heroicons/vue/24/outline"
import {useAuthLazyFetch}  from "../../../composables/useAuthLazyFetch"




//Define the schema of ProjectUrl
interface ProjectFieldUrl{
    ProjectUrl:string,
    url:string
}
// Get the props of ProjectUrl and url
const props = withDefaults(defineProps<ProjectFieldUrl>(), {
    ProjectUrl: "",
    url:""
})

//Showing the project in the table
const getProjectUrl = async () => {
    try {
    const { data: ProjectData } = await useAuthLazyFetch(props.ProjectUrl, {});
    return ProjectData;
  } catch (error) {
    console.error("Error fetching tag URL", error);
    return null;
  }
}
const ProjectUrlData = await getProjectUrl();

 const isProject=ref(false)
////opening the  Project model
const openProject=()=>{
    isProject.value=!isProject.value
}
//saving the Project

const saveProject=(addProjectForm)=>{

useAuthLazyFetchPost(`${props.url}/`, {
    body: {
    name:addProjectForm.name,
    listing_type_name:addProjectForm.listing_type_name,
    category:addProjectForm.category,
    sub_category:"Apartment",
    status:addProjectForm.status,
    details:addProjectForm.details,
    specifications:addProjectForm.specifications,
    possession_date:"2023-04-08",
    age_of_the_project:addProjectForm.age_of_the_project,
    logo_url:addProjectForm.logo_url,
    total_project_area:0,
    metric:"sq.ft",
    default_image_url:addProjectForm.default_image_url,
    visit_count:0,
    rera_approved:true,
    approve_status:addProjectForm.approve_status
    }
  });
}

//Deleting the project
const deleteProjectListener=(project)=>{

if(confirm('Are you sure to delete this record?')){
    useAuthLazyFetchDelete(`${props.url}/${project.uid}`, {});
    let projecturlIndex=ProjectUrlData.value.findIndex(item => item.uid === project.uid)
    if(projecturlIndex!==-1){
      ProjectUrlData.value.splice(projecturlIndex,1)
    }
}
}
//Editing the project

const editProjectListener=(project,editProjectForm)=>{
 useAuthLazyFetchPut(`${props.url}/${project.uid}?name=${editProjectForm.name}&listing_type_name=${editProjectForm.listing_type_name}&category=${editProjectForm.category}&status=${editProjectForm.status}&details=${editProjectForm.details}&specifications=${editProjectForm.specifications}&age_of_the_project=${editProjectForm.age_of_the_project}&logo_url=${editProjectForm.logo_url}&default_image_url=${editProjectForm.default_image_url}&approve_status=${editProjectForm.approve_status}`, {
    body: {
      name:editProjectForm.name,
    listing_type_name:editProjectForm.listing_type_name,
    category:editProjectForm.category,
    sub_category:project.sub_category,
    status:editProjectForm.status,
    details:editProjectForm.details,
    specifications:editProjectForm.specifications,
    possession_date:project.possession_date,
    age_of_the_project:editProjectForm.age_of_the_project,
    logo_url:editProjectForm.logo_url,
    total_project_area:project.total_project_area,
    metric:project.metric,
    default_image_url:editProjectForm.default_image_url,
    visit_count:project.visit_count,
    rera_approved:project.rera_approved,
    approve_status:editProjectForm.approve_status
    },
  });
}

</script>