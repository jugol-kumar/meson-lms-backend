<template>

    <Head title="My Profile" />
    <section class="app-user-profile">
        <div class="row match-height">
            <div class="col-md-6 col-12">
                <div class="card">
                    <div class="card-header border-bottom">
                        <h4 class="card-title">My Profile Details</h4>
                    </div>
                    <div class="card-body py-2 my-25">
                        <!-- header section -->
                        <div class="d-flex">
                            <a href="#" class="me-25">
                                <img :src="imgUrl" id="account-upload-img" class="uploadedAvatar rounded me-50" alt="profile image"
                                     height="100" width="100">
                            </a>
                            <!-- upload and reset button -->
                            <div class="d-flex align-items-end mt-75 ms-1">
                                <div>
                                    <label for="account-upload"
                                           class="btn btn-sm btn-primary mb-75 me-75 waves-effect waves-float waves-light">Upload</label>
                                    <input @input="updateForm.photo = $event.target.files[0]" v-on:change="updateImage" type="file" id="account-upload" hidden="" accept="image/*">
                                    <button @click="resetImage" type="button" id="account-reset"
                                            class="btn btn-sm btn-outline-secondary mb-75 waves-effect">Reset</button>
                                    <p class="mb-0">Allowed file types: png, jpg, jpeg.</p>
                                </div>
                            </div>
                            <!--/ upload and reset button -->
                        </div>
                        <!--/ header section -->

                        <!-- form -->
                        <form class="mt-2 pt-50" @submit.prevent="updateUser">
                            <div class="row">
                                <div class="col-12 col-sm-6 mb-1">
                                    <Text v-model="updateForm.name" type="text" label="Full Name" placeholder="Your full name"
                                          :error="updateForm.errors.name" />
                                </div>
                                <div class="col-12 col-sm-6 mb-1">
                                    <Text v-model="updateForm.phone" type="text" label="Phone" placeholder="Phone number"
                                          :error="updateForm.errors.phone" />
                                </div>
                                <div class="col-12 col-sm-6 mb-1">
                                    <Text v-model="updateForm.email" type="email" label="Email" placeholder="Email Address"
                                          :error="updateForm.errors.email" />
                                </div>
<!--                                <div class="col-12 col-sm-6 mb-1">
                                    <Text v-model="updateForm.married_status" type="text" label="Passport Number" placeholder="Passport Number"
                                          :error="updateForm.errors.married_status" />
                                </div>
                                <div class="col-12 col-sm-6 mb-1">
                                    <label class="form-label">Gender</label>
                                    <select class="form-select" v-model="updateForm.gender">
                                        <option :value="null">Select a gender</option>
                                        <option value="male">Male</option>
                                        <option value="female">Female</option>
                                    </select>
                                </div>
                                <div class="col-12 col-sm-6 mb-1">
                                    <label>Active From: </label>
                                    <datepicker v-model="updateForm.dob" class="form-control" placeholder="Date of Birth" />
                                </div>
                                <div class="col-12 mb-1">
                            <Textarea v-model="updateForm.about" label="About Me" placeholder="About Me"
                                      :error="updateForm.errors.about" />
                                </div>-->
                                <div class="col-12">
                                    <button type="submit"
                                            class="btn btn-primary mt-1 me-1 waves-effect waves-float waves-light">Save
                                        changes</button>
                                </div>
                            </div>
                        </form>
                        <!--/ form -->
                    </div>
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-header border-bottom">
                <h4 class="card-title">Change Password</h4>
            </div>
            <div class="card-body py-2 my-25">
                <div v-if="$page.props.flash.message" class="text-danger">
                    <p class="text-danger">{{ $page.props.flash.message }}</p>
                </div>
                <!-- form -->
                <form class="mt-2 pt-50" @submit.prevent="changePassword">
                    <div class="row">
                        <div class="col-12 col-sm-4 mb-1">
                            <Text v-model="resetPassword.oldPassword" type="password" label="Current Password" placeholder="Current Password"
                                  :error="resetPassword.errors.oldPassword" />
                        </div>
                        <div class="col-12 col-sm-4 mb-1">
                            <Text v-model="resetPassword.newPassword" type="password" label="New Password" placeholder="New Password"
                                  :error="resetPassword.errors.newPassword" />
                        </div>
                        <div class="col-12 col-sm-4 mb-1">
                            <Text v-model="resetPassword.retypePassword" type="password" label="Retype Password" placeholder="Retype New Password"
                                  :error="resetPassword.errors.retypePassword" />
                        </div>
                        <div class="col-12">
                            <button type="submit"
                                    class="btn btn-primary mt-1 me-1 waves-effect waves-float waves-light">Save
                                changes</button>
                        </div>
                    </div>
                </form>
                <!--/ form -->
            </div>
        </div>
    </section>
</template>

<script>
import SLayout from './Layout.vue'
export default {
    layout: SLayout,
};
</script>

<script setup>
import { ref } from 'vue'
import Datepicker from 'vue3-datepicker'
import Textarea from '@/Components/form/Textarea.vue'
import Text from '@/Components/form/Text.vue'
import Swal from "sweetalert2";
import {useForm} from "@inertiajs/vue3";

let props = defineProps({
    user: Object,
})

let updateForm = useForm({
    name: props.user.name,
    email: props.user.email,
    phone: props.user.phone,
    dob: props.user.dob,
    gender: props.user.gender,
    married_status: props.user.married_status,
    photo: '',
    // address: '',
    // city_id: '',
    // state_id: '',
    // country_id: '',
    about: props.user.about,
})


let resetPassword = useForm({
    oldPassword:'',
    newPassword:'',
    retypePassword:'',
    errors:""
})

let changePassword = () =>{
    resetPassword.post("/reset-password",{
        preserveScroll:true,
        onError:(res)=>{
            Swal.fire({
                icon:'error',
                title: "Error !",
                text: props.getError.msg
            })
        },
        onSuccess: (res) => {
            Swal.fire({
                icon:'error',
                title: "Error !",
                text: props.getError.msg
            })
        }
    });
}




let updateUser = () => {
    updateForm.post('', {
        onSuccess: () => console.log('Saved')
    });
}

let imgUrl = ref(props.user.photo);

const updateImage = (event) => {
    imgUrl.value = URL.createObjectURL(event.target.files[0])
}

const resetImage = () => {
    imgUrl.value = props.user.photo
    updateForm.reset('photo')
}

</script>
