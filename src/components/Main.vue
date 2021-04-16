<template>
  <div class="__container">
     
      <div class="__search">
        <span class="caption">Search ICD Code</span>
        <b-input v-model="icdCode" class="txtSearch" placeholder="Search Code..." rounded></b-input>
        <b-button @click="onSearchClick" title="Search ICD Code" type="is-primary"
        icon-left="search">
            Search
        </b-button>
      </div>

      <div v-if="icds.length > 0" class="__result">
        <article :key="index" v-for="(icd, index) in icds" class="panel is-info">
            <p class="panel-heading __bg">
                {{ icd.Name }}
            </p>
            
            <div class="panel-block">
                <span class="panel-icon">
                <i class="fas fa-book" aria-hidden="true"></i>
                </span>
                Description: {{ icd.Description }}
            </div>
        </article>
      </div>

      <div v-else class="__noresult">
        <p style="font-weight: 600;font-size: 2.5rem;">
            No Records Found.
        </p> 
      </div>

    <b-loading :is-full-page="isFullPage" v-model="isLoading" :can-cancel="false"></b-loading>

  </div>
</template>

<script>

import axios from 'axios'

export default {
    data() {
        return {
            baseURL: 'https://clinicaltables.nlm.nih.gov/api/icd10cm/v3/search?',
            icds: [],
            isLoading: false,
            isFullPage: true,
            icdCode: '',
            icd: {
                Name: '',
                Description: ''
            }
        }
    },
    methods: {
        onSearchClick() {            

            this.isLoading = true
            axios.get(this.baseURL+'terms='+this.icdCode).then((res)=> {
                
                this.icds = []              

                if(res.data[3].length > 0) {
                   
                    const _icds  = res.data[3]
                    _icds.forEach((obj) => {
                        
                        console.log(obj)

                        const code = {
                            Name: obj[0],
                            Description: obj[1] 
                        }

                        // this.icd.Name = obj[0]
                        // this.icd.Description = obj[1]
                        this.icds.push(code)

                    })

                }              

               console.log(res.data[3])
                this.isLoading = false

            })
            .catch((error) => console.log(error));
        }
    }
}
</script>

<style scoped>

.__container {
    padding: 20px;
    height: 100vh;
}

.__search {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    align-items: center;    
    gap: 15px;
}

.__search > .caption {
    font-weight: 600;
}

.__search > .txtSearch  {
    width: 550px;
}

.__result,
.__noresult {
    margin-top: 50px;
    padding: 0 300px 0 300px;
}

.__bg {
    background-color: #2d3f52 !important;
}



</style>