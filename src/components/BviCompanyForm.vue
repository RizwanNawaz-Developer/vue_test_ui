<template>
    
    <div class="container bvi-container">
        <!-- Stepper -->
        <div class="d-flex justify-content-between mb-4">
            <div v-for="(step, index) in steps" :key="index" class="text-center">
                <div
                    :class="['step-circle', index === activeStep ? 'bg-primary text-white' : 'bg-secondary text-white-50']"
                >
                    {{ index + 1 }}
                </div>
                <span :class="index === activeStep ? 'step-circle-text text-primary fw-semibold ms-2' : 'step-circle-text text-secondary ms-2'">{{ step }}</span>
            </div>
        </div>

        <form @submit.prevent="handleSubmit" class="g-3 needs-validation" novalidate>
            <!-- point of contact section -->
            <div class="form-section">
                <!-- <h5 class="fw-bold text-white border-bottom border-2 border-primary ">Point of contact</h5> -->
                <div class="section-title">Point of contact</div>
                <div class="row pt-5">
                    <div class="col-md-5">
                        <p class="text-white w-75 ps-3">
                            This is the individual that we will communicate with. Communications related to this form but also to the company
                            once incorporated will be sent to the same email address. You can change it later on if required.
                        </p>
                    </div>
                    <div class="col-md-5">
                        <div class="mb-3">
                            <label for="fullName" class="form-label">Full Name</label>                            
                            <input 
                                type="text"
                                class="form-control"
                                :class="{
                                    'is-invalid': errors.fullName,
                                    'is-valid': !errors.fullName && form.fullName.length > 2
                                }"
                                v-model="form.fullName" 
                                id="fullName" 
                                @input="clearError('fullName')"
                                required 
                            />
                            <div v-if="errors.fullName" class="invalid-feedback">
                                {{ errors.fullName }}
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="email" class="form-label">Email</label>
                            <input 
                                type="email" 
                                v-model="form.email"
                                class="form-control" 
                                id="email"
                                :class="{
                                    'is-invalid': errors.email,
                                    'is-valid': !errors.email && isValidEmail(form.email)
                                }" 
                                @input="clearError('email')"
                                required
                            />
                            <div v-if="errors.email" class="invalid-feedback">
                                {{ errors.email }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!-- company information section -->
            <div class="form-section">
                <div class="section-title">Company information</div>
                <div class="row pt-5">
                    <div class="col-md-5">
                        <p class="text-white w-75 ps-3">
                            Every company must have a name and a designation. For the company name you can use both letters and numbers, but not special symbols. For the designations, there is no actual different between one or another.
                        </p>
                    </div>
                    <div class="col-md-5">
                        <div class="mb-3">
                            <label for="companyName" class="form-label">Company name</label>
                            <input 
                                type="text" 
                                class="form-control" 
                                v-model="form.companyName" 
                                placeholder="The name you want your company to have" 
                                id="companyName" 
                                :class="{
                                    'is-invalid': errors.companyName,
                                    'is-valid': !errors.companyName && form.companyName.length > 2
                                }"
                                @input="clearError('companyName')"
                                required
                            />
                            <div v-if="errors.companyName" class="invalid-feedback">
                                {{ errors.companyName }}
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="altcompanyName" class="form-label">Alternative company name</label>
                            <input 
                                type="text" 
                                class="form-control" 
                                v-model="form.altcompanyName" 
                                placeholder="The name to use if the first name is not available" 
                                id="altcompanyName"
                                :class="{
                                    'is-invalid': errors.altcompanyName,
                                    'is-valid': !errors.altcompanyName && form.altcompanyName.length > 2
                                }"
                                @input="clearError('altcompanyName')"
                                required
                            />
                            <div v-if="errors.altcompanyName" class="invalid-feedback">
                                {{ errors.altcompanyName }}
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="companyDesignation" class="form-label">Company designation</label>
                            <select 
                                class="form-select" 
                                id="companyDesignation" 
                                v-model="form.companyDesignation"
                                :class="{
                                    'is-invalid': errors.companyDesignation,
                                    'is-valid': !errors.companyDesignation && form.companyDesignation.length > 2
                                }"
                                @input="clearError('companyDesignation')"
                                required
                            >
                                <option value="" selected>Select the option that you prefer</option>
                                <option value="">First Option</option>
                            </select>
                            <div v-if="errors.companyDesignation" class="invalid-feedback">
                                {{ errors.companyDesignation }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!-- countries of interest section -->
            <div class="form-section">
                <div class="section-title">Countries of interest</div>
                <div class="row pt-5">
                    <div class="col-md-5">
                        <p class="text-white w-75 ps-3">
                            We are required to check that the company will not be interacting with forbidden locations. For juridiction of operation If you are alone, select your country of residency. If you have more shareholders, pick the most relevant. For 'target juridiction' select 1-3 countries that are relevant. Even if you will have clients from other countries, it's ok.
                        </p>
                    </div>
                    <div class="col-md-5">
                        <div class="mb-3">
                            <label for="juridiction" class="form-label">Jurisdiction of operation</label>
                            <select 
                                class="form-select" 
                                id="juridiction" 
                                v-model="form.juridiction"
                                :class="{
                                    'is-invalid': errors.juridiction,
                                    'is-valid': !errors.juridiction && form.juridiction.length > 2
                                }"
                                @input="clearError('juridiction')"
                                required
                            >
                                <option value="" selected>Select the country where you are located</option>
                                <option value="">First Option</option>
                            </select>
                            <div v-if="errors.juridiction" class="invalid-feedback">
                                {{ errors.juridiction }}
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="target" class="form-label">Target jurisdictions</label>
                            <select 
                                class="form-select" 
                                id="target" 
                                v-model="form.target"
                                :class="{
                                    'is-invalid': errors.target,
                                    'is-valid': !errors.target && form.target.length > 2
                                }"
                                @input="clearError('target')"
                                required
                            >
                                <option value="" selected>Select the country where your clients are located</option>
                                <option value="">First Option</option>
                            </select>
                            <div v-if="errors.target" class="invalid-feedback">
                                {{ errors.target }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!-- shares structure section -->
            <div class="form-section">
                <div class="section-title">Shares structure</div>
                <div class="row pt-5">
                    <div class="col-md-5">
                        <p class="text-white w-75 ps-3">
                            All companies must have at least 1 share. Apart from that, you can structure things in whatever way you like. Issued shares and shares that will be distributed from day 1. Unissued shares are shares that you can distribute later on, i.e. to future team members or investors. The value per shares represents your personal liability, so, if you wish to reduce risks, just pick the smallest number.
                        </p>
                    </div>
                    <div class="col-md-5">
                        <div class="mb-3">
                            <label for="shares" class="form-label">Number of shares</label>
                            <select 
                                class="form-select" 
                                id="shares" 
                                v-model="form.shares"
                                :class="{
                                    'is-invalid': errors.shares,
                                    'is-valid': !errors.shares && form.shares.length > 2
                                }"
                                @input="clearError('shares')"
                                required
                            >
                                <option value="" selected>Select how many shares you wish to have</option>
                                <option value="">First Option</option>
                            </select>
                            <div v-if="errors.shares" class="invalid-feedback">
                                {{ errors.shares }}
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="target" class="form-label">Are all shares issued?</label>
                            <div class="radio-group">
                                <label for="yes">
                                    <input 
                                        type="radio" 
                                        name="issued" 
                                        value="yes" 
                                        required
                                        id="yes" 
                                        v-model="form.issued"
                                        :class="{ 
                                            'is-invalid': errors.issued, 
                                            'is-valid': !errors.issued
                                        }"
                                    /> 
                                        Yes
                                </label>
                                <label for="no">
                                    <input 
                                        type="radio" 
                                        name="issued" 
                                        value="no" 
                                        required 
                                        id="no"
                                        v-model="form.issued"
                                        :class="{ 
                                            'is-invalid': errors.issued, 
                                            'is-valid': !errors.issued
                                        }"
                                    /> 
                                        No
                                </label>
                            </div>
                            <div v-if="errors.issued" class="invalid-feedback">
                                {{ errors.issued }}
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="number_share" class="form-label">Number of issued shares</label>
                            <input 
                                type="number" 
                                class="form-control" 
                                v-model="form.number_share" 
                                id="number_share" 
                                placeholder="Write how many shares you wish to issue per day"
                                :class="{
                                    'is-invalid': errors.number_share,
                                    'is-valid': !errors.number_share && form.number_share >= 100
                                }"
                                @input="clearError('number_share')"
                                required 
                            />
                            <div v-if="errors.number_share" class="invalid-feedback">
                                {{ errors.number_share }}
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="values_share" class="form-label">Value per share</label>
                            <select 
                                class="form-select" 
                                id="values_share" 
                                v-model="form.values_share"
                                :class="{
                                    'is-invalid': errors.values_share,
                                    'is-valid': !errors.values_share && form.values_share.length > 2
                                }"
                                @input="clearError('values_share')"
                                required
                            >
                                <option value="" selected>Select how much each share is worth</option>
                                <option value="">First Option</option>
                            </select>
                            <div v-if="errors.values_share" class="invalid-feedback">
                                {{ errors.values_share }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="d-flex justify-content-between mt-5">
                <button type="button" class="form-btn">
                    <i class="bi bi-arrow-left"></i>
                    BACK
                </button>
                <button type="submit" class="form-btn forn-btn-center">
                    I know the form autosaved but I wish to<br /> <b>SAVE & EXIT</b> anyway. :)
                </button>
                <button type="button" class="form-btn form-btn-right">
                    NEXT
                    <i class="bi bi-arrow-right"></i>
                </button>
            </div>
        </form>
    </div>
    
</template>

<script setup>

    import { ref, computed } from 'vue';
   // import * as Yup from 'yup';

    // Step titles
    const steps = [
        'Company Details',
        'Shareholders',
        'Beneficial Owner',
        'Director'
    ]

    const activeStep = ref(0)

    const form = ref({
        fullName: '',
        email: '',
        companyName: '',
        altcompanyName: '',
        companyDesignation: '',
        juridiction: '',
        target: '',
        shares: '',
        issued: '',  
        number_share: '',
        values_share: ''
    })
    

    const errors = ref({})

    // simple email regex
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/

    function handleSubmit() {
        errors.value = {} // Reset previous errors

        if (!form.value.fullName) {
            errors.value.fullName = 'Full Name is required'
        } else if (form.value.fullName.length < 2) {
            errors.value.fullName = 'Full name must be at least 2 characters'
        }

        if (!form.value.email) {
            errors.value.email = 'Email is required'
        } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.value.email)) {
            errors.value.email = 'Invalid email format'
        }

        if (!form.value.companyName) {
            errors.value.companyName = 'Company name is required'
        } else if (form.value.companyName.length < 2) {
            errors.value.companyName = 'company name must be at least 2 characters'
        }

        if (!form.value.altcompanyName) {
            errors.value.altcompanyName = 'Alternative company name is required'
        } else if (form.value.altcompanyName.length < 2) {
            errors.value.altcompanyName = 'Alternative company name must be at least 2 characters'
        }

        if (!form.value.companyDesignation) {
            errors.value.companyDesignation = 'Company designation is required'
        } else if (form.value.companyDesignation.length < 2) {
            errors.value.companyDesignation = 'Company designation must be at least 2 characters'
        }

        if (!form.value.juridiction) {
            errors.value.juridiction = 'Juridiction is required'
        } else if (form.value.juridiction.length < 2) {
            errors.value.juridiction = 'Juridiction must be at least 2 characters'
        }

        if (!form.value.target) {
            errors.value.target = 'Target is required'
        } else if (form.value.target.length < 2) {
            errors.value.target = 'Target must be at least 2 characters'
        }

        if (!form.value.shares) {
            errors.value.shares = 'Shares is required'
        } else if (form.value.shares.length < 2) {
            errors.value.shares = 'Shares must be at least 2 characters'
        }

        if (!form.value.issued) {
            errors.value.issued = 'Please select shares issued'
        }

        if (!form.value.number_share) {
            errors.value.number_share = 'Please enter number of shares'
        } else if(form.value.number_share < 100) {
            errors.value.number_share = 'Number of shares must be 100 or greater'
        }

        if (Object.keys(errors.value).length === 0) {
            alert('Form is valid and submitted!')
        }

        if (!form.value.values_share) {
            errors.value.values_share = 'Share value is required'
        } else if (form.value.values_share.length < 2) {
            errors.value.values_share = 'Share value must be at least 2 characters'
        }

    }

    function isValidEmail(value) {
        return emailRegex.test(value)
    }

    function clearError(field) {
        if (errors.value[field]) {
            delete errors.value[field]
        }
    }

</script>
