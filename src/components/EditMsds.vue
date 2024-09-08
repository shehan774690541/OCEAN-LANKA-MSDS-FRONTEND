<template>
    <div class="container">
        <h2 style="text-align: center;">
            Update MSDS {{ indexNb }}
        </h2>
        Name Of MSDS<input type="text" v-model="name"
            style="width: 100%; padding: 5px; border-radius: 8px; border: 1px solid;">
        <br> <br>

        Select Your PDF <br>
        <input type="file" @change="logFileContent" @ref="pdfFile" /> <br><br>

        Departments
        <div class="checkbox-deps">
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="Dyeing" id="depDyeing"> <label
                    for="depDyeing">Dyeing</label>
            </div>
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="Finishing" id="depFinishing"> <label
                    for="depFinishing">Finishing</label>
            </div>
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="Knitting" id="depKnitting"> <label
                    for="depKnitting">Knitting</label>
            </div>
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="Lab Dip" id="depLabdip"> <label for="depLabdip">Lab
                    Dip</label>
            </div>
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="ETP" id="DepEtp"> <label for="DepEtp">ETP</label>
            </div>
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="Priting" id="DepPrinting"> <label
                    for="DepPrinting">Printing</label>
            </div>
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="Testing Lab" id="DepTestinglab"> <label
                    for="DepTestinglab">Testing Lab</label>
            </div>
            <div class="checkbox-item">
                <input type="checkbox" v-model="deps" value="Chemical W.H." id="DepChemicalwh"> <label
                    for="DepChemicalwh">Chemical W.H</label>
            </div>
        </div>

        <!-- Submit Buttons -->
        <div class="submit-buttons">
            <button v-on:click="clearForm()">Clear</button>
            <button v-on:click="createMsds()">Create</button>
        </div>

    </div>
</template>
<script>
export default {
    name: 'newMsds',
    props: {
        indexNb: String
    },
    data() {
        return {
            name: "",
            fileContent: '',
            deps: [],
        }
    },
    methods: {
        createMsds() {
            if(this.name != "" && this.fileContent != ''){
                alert("you can upload")
            }else{
                alert("check the inputs")
            }
            // console.log("Name : ", this.name)
            // console.log("File : ", this.fileContent)
            // console.log("Dept : ", this.deps)
            // console.log(this.fileContent)
            // this.clearForm()
        },
        clearForm() {
            this.name = ""
            this.fileContent = '';
            this.deps = []

        },
        logFileContent(event) {
            const file = event.target.files[0];

            if (event.target.files[0].type == "application/pdf") {
                if (event.target.files[0].size <= 3912088) {
                    if (file) {
                        const reader = new FileReader();
                        reader.onload = (e) => {
                            this.fileContent = e.target.result;
                            // console.log(this.fileContent);
                        };
                        reader.readAsText(file);
                    } else {
                        console.log('No file selected');
                    }
                } else {
                    console.log('Maximum Size is 3mb. can\' upload this file');
                }
            } else {
                console.log('This Is Not a PDF file');
            }

        }

    }
}

</script>
<style>
.container {
    width: 100%;
    height: 100%;
}

.checkbox-deps {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 100%;
    max-width: 300px;
    border: 1px solid;
    padding: 5px;
    border-radius: 10px;
}

.checkbox-item {
    flex: 1 1 30%;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
}

.checkbox-item label {
    margin-left: 5px;
}

.submit-buttons {
    margin: 11px;
    width: 100%;
    display: flex;
    justify-content: space-around;
}

.submit-buttons button {
    padding: 5px;
    border: 1px solid;
    border-bottom-color: #ffffff;
}
</style>