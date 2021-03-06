<template>
    <form id="registration-form" @submit.prevent="signUpPressed">
        <div class="wrapper">
            <div class="form-control">
                <label for="first-name">First Name</label>
                <input type="text"
                    v-model="firstName" 
                    name="first-name" 
                    @focus="inFocus('firstName')" 
                    @blur="outFocus('firstName')" 
                    :class="getClass('firstName')" 
                    :placeholder="getPlaceholder('firstName')">
                <div class="alert-info" 
                    v-if="!isFocused('firstName') 
                    && !($v.firstName.minLength 
                    && $v.firstName.maxLength)"
                    >
                    First name must be 2 to 20 characters long.
                </div>
            </div>
            <div class="form-control">
                <label for="last-name">Last Name</label>
                <input type="text" 
                    v-model="lastName" 
                    name="last-name" 
                    @focus="inFocus('lastName')" 
                    @blur="outFocus('lastName')" 
                    :class="getClass('lastName')" 
                    :placeholder="getPlaceholder('lastName')">
                <div class="alert-info" 
                    v-if="!isFocused('lastName') && 
                    !($v.lastName.minLength 
                    && $v.lastName.maxLength)"
                    >
                    Last name must be 2 to 20 characters long.
                </div>
            </div>
        </div>
        <div class="wrapper">
            <div class="form-control">
                <label for="email">Email</label>
                <input type="text" 
                    v-model="email" 
                    name="email" 
                    @focus="inFocus('email')" 
                    @blur="outFocus('email')" 
                    :class="getClass('email')" 
                    :placeholder="getPlaceholder('email','example@rentr.com')">
                <div class="alert-info" 
                    v-if="!isFocused('email') 
                    && !$v.email.email"
                    >
                    Incorrect email format.
                </div>
            </div>
            <div class="form-control">
                <label for="phone-number">Phone Number</label>
                <PhoneNumberInput
                    class="phone-number" 
                    v-model="phoneTmp" 
                    default-country-code="RS" 
                    size="lg"
                    color="#f0a500"
                    valid-color="green"
                    error-color="red"
                    @update="updatePhone"/>
                <div class="alert-info" 
                    v-if="phone && !phone.isValid">
                    Incorrect phone number.
                </div>
            </div>
        </div>
        <div class="wrapper">
            <div class="form-control">
                <label for="password">Password</label>
                <input type="password" 
                    v-model="password" 
                    name="password"  
                    @focus="inFocus('password')"
                    @blur="outFocus('password')" 
                    :class="getClass('password')"
                    :placeholder="getPlaceholder('password', 'At least 8 characters')">
                <div class="alert-info" 
                    v-if="!isFocused('password') 
                    && !($v.password.minLength 
                    && $v.password.maxLength)"
                    >
                    Password must be 8 to 30 characters long.
                </div>
            </div>
            <div class="form-control">
                <label for="confirm-password">Confirm password</label>
                <input type="password" 
                    v-model="confirmPassword" 
                    name="confirm-password" 
                    @focus="inFocus('confirmPassword')" 
                    @blur="outFocus('confirmPassword')" 
                    :class="getClass('confirmPassword')" 
                    :placeholder="getPlaceholder('confirmPassword')">
                <div class="alert-info" 
                    v-if="!isFocused('confirmPassword') 
                    && !$v.confirmPassword.sameAsPassword"
                    >
                    Passwords don't match.
                </div>
            </div>
        </div>
        <AddressInput
            @update:address="updateAddress"
            @update:city="updateCity"
            @update:country="updateCountry"
            :validate="true"/>
        <div class="wrapper">
            <div class="form-control">
                <label for="datepicker">Date of Birth (Min. Age: 18)</label>
                <DropdownDatepicker
                    id="datepicker" 
                    name="datepicker" 
                    displayFormat="dmy" 
                    :minAge="18"
                    :onChange="updateDate"
                    :onDayChange="updateDay"
                    :onMonthChange="updateMonth"
                    :onYearChange="updateYear"
                    />
                <div class="alert-info"
                    v-if="!isFocused('dateOfBirth') 
                    && !this.dateOfBirth.isValid"
                    >
                    Select a valid date.
                </div>
            </div>
            <div class="form-control">
                <label for="role" class="block-label">Account Type</label>
                <select name="role" id="role" @click="infocus.role = false;" v-model="role">
                    <option value="" selected disabled hidden>Choose account type</option>
                    <option value="HOUSE_OWNER">Vacation Home Owner</option>
                    <option value="SHIP_OWNER">Ship Owner</option>
                    <option value="FISHING_INSTRUCTOR">Fishing Instructor</option>
                </select>
                <div class="alert-info"
                    v-if="!isFocused('role') && this.role === ''">
                    Select an account type.
                </div>
            </div>
        </div>
        <div class="wrapper" id="registration-reason">
             <div class="form-control">
                <label for="description" class="block-label">Reason for Registration</label>
                <textarea name="description" 
                    id="description" cols="30" rows="4"
                    v-model="registrationReason" 
                    @focus="inFocus('registrationReason')" 
                    @blur="outFocus('registrationReason')" 
                    :class="getClass('registrationReason')" 
                    :placeholder="getPlaceholder('registrationReason', 'Tell us a few reasons why you want to join...')"/>
                <div class="alert-info" 
                    v-if="!isFocused('registrationReason')
                    && !($v.registrationReason.minLength 
                    && $v.registrationReason.maxLength)"
                    >
                    Enter a registration reason.
                </div>
            </div>
        </div>
        <div class="btn-div">
            <button class="btn"
                :disabled="$v.$invalid 
                || (!phone || !phone.isValid) 
                || !dateOfBirth.isValid"
                >
                Create Account
            </button>
            <div class="already-registered">
                Already have an account? 
                <router-link to="/login">
                    Log in
                </router-link>
            </div>
        </div>
    </form>
</template>
<script>
import { required, minLength, maxLength, sameAs, email } from 'vuelidate/lib/validators'
import DropdownDatepicker from 'vue-dropdown-datepicker/src/dropdown-datepicker.vue';
import PhoneNumberInput from 'vue-phone-number-input';
import AddressInput from './AddressInput.vue';
import axios from 'axios';

export default {
    name: 'RegistrationFrom',
    components: {
        DropdownDatepicker,
        PhoneNumberInput,
        AddressInput,
    },
    data() {
        return {
            firstName: '',
            lastName: '',
            email: '',
            phone: null,
            password: '',
            confirmPassword: '',
            address: '',
            city: '',
            country: '',
            registrationReason: '',
            role: '',
            phoneTmp: '',
            dateOfBirth:{
                day: null,
                month: null,
                year: null,
                isValid: false,
                updatingDay: false,
            },
            infocus: {
                firstName: true,
                lastName: true,
                username: true,
                email: true,
                password: true,
                confirmPassword: true,
                registrationReason: true,
                role: true,
                dateOfBirth: true,
            }
        }
    },
    validations:{
        firstName: {
            required,
            minLength: minLength(2),
            maxLength: maxLength(20)
        },
        lastName: {
            required,
            minLength: minLength(2),
            maxLength: maxLength(20)
        },
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(8),
            maxLength: maxLength(30)
        },
        confirmPassword: {
            required,
            sameAsPassword: sameAs("password")
        },
        registrationReason: {
            required,
            minLength: minLength(20),
            maxLength: maxLength(200),
        },
        address: {
            required,
            minLength: minLength(5),
            maxLength: maxLength(40)
        },
        city: {
            required,
            minLength: minLength(2),
            maxLength: maxLength(40)
        },
        country: {
            required
        },
        role: {
            required,
        },
    },
    methods: {
        signUpPressed(){
            let day = (""+this.dateOfBirth.day).length === 1 ? "0"+this.dateOfBirth.day : ""+this.dateOfBirth.day
            let month = (""+this.dateOfBirth.month).length === 1 ? "0"+this.dateOfBirth.month : ""+this.dateOfBirth.month
            let dob = ""+day+"."+month+"."+this.dateOfBirth.year+"."
            let registrationRequestDTO = {
                    firstName: this.firstName,
                    lastName: this.lastName,
                    email: this.email,
                    password: this.password,
                    role: this.role,
                    phoneNumber: this.phone.formattedNumber,
                    dateOfBirth: dob,
                    address: this.address,
                    city: this.city,
                    country: this.country,
                    registrationReason: this.registrationReason
                }
            console.log(registrationRequestDTO)
            axios
                .post(process.env.VUE_APP_BASE_URL+"/api/v1/registration/business-client", registrationRequestDTO)
                .then(function(response) {
                    console.log(response)
                    alert("Registration successfull. You will receive an answer from our admins shortly.")
                })
                .catch(function(error) {
                    console.log(error);
                    alert("Email already taken");
                })
        },
        updatePhone(event){
            this.phone = event
        },
        isDateValid(){
            if(this.dateOfBirth.day && this.dateOfBirth.month && this.dateOfBirth.year)
                this.dateOfBirth.isValid = true;
        },
        updateDay(event){
            this.dateOfBirth.updatingDay = true;
            this.dateOfBirth.day = event;
            this.isDateValid();
        },
        updateMonth(event){
            this.dateOfBirth.month = event;
            this.isDateValid();
        },
        updateYear(event){
            this.dateOfBirth.year = event;
            this.isDateValid();
            this.infocus.dateOfBirth = false;
        },
        updateDate(day){
            if(this.dateOfBirth.updatingDay){
                this.dateOfBirth.updatingDay = false;
            }
            else if (day != this.dateOfBirth.day) {
                this.dateOfBirth.day = null;
                this.dateOfBirth.isValid = false;
            }
        },
        updateAddress(event){
            this.address = event;
        },
        updateCity(event){
            this.city = event;
        },
        updateCountry(event){
            this.country = event;
        },
        isFocused(field) {
            return this.infocus[field]
        },
        inFocus(field) {
            this.infocus[field] = true
        },
        outFocus(field) {
            this.infocus[field] = false
        },
        getClass(field) {
            let cls = !this.isFocused(field) && this.$v[field].$invalid ? 'alert' : '';
            return cls;
        },
        getPlaceholder(field, defaultPlaceholder='') {
            let placeholder = !this.isFocused(field) && this.$v[field].$invalid ? 'Required' : defaultPlaceholder;
            return placeholder;
        }
    },
}
</script>

<style scoped>

.form-control {
    margin: 15px 10px 15px 0px;
}

.form-control:last-child {
    margin-right: 0px;
}

.wrapper {
    display: grid;
    grid-template-columns: 50% 50%;
}

.address-wrapper {
    grid-template-columns: 50% 30% 20%;
}

textarea {
    height: auto;
    width: 100%;
}

.form-control input, select {
    width: 100%;
    height: 48px;
}

.form-control label {
    width: 100%;
}

.form-control {
    display: block;
}

#datepicker {
    display: grid;
    grid-template-columns: 33.3% 33.3% 33.3%;
}

#registration-reason {
    display: block;
}

.btn-div {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: 20px;
}

.btn {
    width: 40%;
}

.btn-div p {
    margin-top: 0px;
}

.already-registered {
    font-size: 0.9rem !important;
    margin-top: 5px;
}
</style>