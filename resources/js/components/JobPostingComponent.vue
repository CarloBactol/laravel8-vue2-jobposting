<template>
    <div>
        <h1>JobPosting List</h1>
        <table class="table table-hover">
            <thead>
                <th>Job Name</th>
                <th>Job Description</th>
                <th>Vacants</th>
                <th>Actions</th>
            </thead>
            <tbody>
                <tr v-for="(item, index) in postingList" :key="item.id">
                    <td>{{ item.job_name }}</td>
                    <td>
                        {{ item.job_description }}
                    </td>
                    <td>{{ item.vacants }}</td>
                    <td>
                        <button
                            class="btn btn-sm btn-secondary"
                            @click="edit(item)"
                        >
                            <i class="fa fa-edit"></i>
                        </button>
                        <button
                            class="btn btn-sm btn-danger"
                            @click="remove(item, index)"
                        >
                            <i class="fa fa-trash"></i>
                        </button>
                    </td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td>
                        <div class="form-group">
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Input Job Name"
                                v-model="form.job_name"
                            />
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Input Job Description"
                                v-model="form.job_description"
                            />
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <input
                                type="number"
                                class="form-control"
                                placeholder="e.g: 10"
                                v-model="form.vacants"
                            />
                        </div>
                    </td>
                    <td>
                        <button class="btn btn-info" @click="submit">
                            <i class="fa fa-plus"></i>
                        </button>
                    </td>
                </tr>
            </tfoot>
        </table>
        <!-- Modal -->
        <div
            class="modal fade"
            id="editModal"
            tabindex="-1"
            aria-labelledby="exampleModalLabel"
            aria-hidden="true"
        >
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">
                            Modal title
                        </h5>
                        <button
                            type="button"
                            class="btn-close"
                            data-bs-dismiss="modal"
                            aria-label="Close"
                        ></button>
                    </div>
                    <div class="modal-body">
                        <div class="form-group">
                            <label for="job_name">Job Name</label>
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Input Job Description"
                                v-model="formEdit.job_name"
                            />
                        </div>
                        <div class="form-group">
                            <label for="job_description">Job Description</label>
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Input Job Description"
                                v-model="formEdit.job_description"
                            />
                        </div>
                        <div class="form-group">
                            <label for="vacants">Vacants</label>
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Input Job Description"
                                v-model="formEdit.vacants"
                            />
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button
                            type="button"
                            class="btn btn-secondary"
                            data-bs-dismiss="modal"
                        >
                            Close
                        </button>
                        <button
                            type="button"
                            class="btn btn-primary"
                            @click="save()"
                        >
                            Save changes
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    //get data from parents to child
    props: ["posting"],
    data() {
        return {
            postingList: this.posting,
            form: {
                job_name: null,
                job_description: null,
                vacants: 0,
            },
            formEdit: {
                job_name: null,
                job_description: null,
                vacants: 0,
            },
            selectedId: null,
        };
    },
    methods: {
        submit() {
            //vm to create varaible for this
            const vm = this;
            axios
                .post("/job_postings", this.form)
                .then(function (response) {
                    vm.postingList.push(response.data.data);
                })
                .catch((error) => console.log(error));
        },
        remove(item, index) {
            const vm = this;
            axios
                .delete(`/job_postings/${item.id}`)
                .then(function (response) {
                    vm.postingList.splice(index, 1);
                })
                .catch((error) => console.log(error));
        },
        edit(item) {
            const vm = this;
            $("#editModal").modal("show");
            vm.formEdit.job_name = item.job_name;
            vm.formEdit.job_description = item.job_description;
            vm.formEdit.vacants = item.vacants;
            vm.selectedId = item.id;
        },
        save() {
            const vm = this;
            axios
                .put(`/job_postings/${vm.selectedId}`, this.formEdit)
                .then(function (response) {
                    alert("Successfully save!");
                    location.reload();
                })
                .catch((error) => console.log(error));
        },
    },
};
</script>
