<template>
    <jet-form-section @submitted="updateTeamName">
        <template #title>
            Team Name
        </template>

        <template #description>
            The team's name and owner information.
        </template>

        <template #form>
            <!-- Team Owner Information -->
            <div class="mb-4">
                <jet-label value="Team Owner" />

                <div class="d-flex items-center mt-2">
                    <img class="rounded-circle" width="48" :src="team.owner.profile_photo_url" :alt="team.owner.name">

                    <div class="ml-2">
                        <div>{{ team.owner.name }}</div>
                        <div class="text-muted">{{ team.owner.email }}</div>
                    </div>
                </div>
            </div>

            <!-- Team Name -->
            <div class="w-75">
                <jet-label for="name" value="Team Name" />

                <jet-input id="name"
                            type="text"
                            class="mt-1 block w-full"
                           :class="{ 'is-invalid': form.error('name') }"
                            v-model="form.name"
                            :disabled="! permissions.canUpdateTeam" />

                <jet-input-error :message="form.error('name')" />
            </div>
        </template>

        <template #actions v-if="permissions.canUpdateTeam">
            <jet-action-message :on="form.recentlySuccessful" class="mr-3">
                Saved.
            </jet-action-message>

            <jet-button :class="{ 'text-black-50': form.processing }" :disabled="form.processing">
                Save
            </jet-button>
        </template>
    </jet-form-section>
</template>

<script>
    import JetActionMessage from './../../Jetstream/ActionMessage'
    import JetButton from './../../Jetstream/Button'
    import JetFormSection from './../../Jetstream/FormSection'
    import JetInput from './../../Jetstream/Input'
    import JetInputError from './../../Jetstream/InputError'
    import JetLabel from './../../Jetstream/Label'

    export default {
        components: {
            JetActionMessage,
            JetButton,
            JetFormSection,
            JetInput,
            JetInputError,
            JetLabel,
        },

        props: ['team', 'permissions'],

        data() {
            return {
                form: this.$inertia.form({
                    name: this.team.name,
                }, {
                    bag: 'updateTeamName',
                    resetOnSuccess: false,
                })
            }
        },

        methods: {
            updateTeamName() {
                this.form.put('/teams/' + this.team.id, {
                    preserveScroll: true
                });
            },
        },
    }
</script>
