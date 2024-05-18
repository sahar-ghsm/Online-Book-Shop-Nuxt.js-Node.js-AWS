<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="text-align: center">Update {{product.title}}</h2>
            <form>
              <div class="a-spacing-top-medium">
                <label>Category</label>
                <select class="a-select-option" v-model="categoryID">
                  <option v-for="category in categories" :value="category._id" :key="category._id">{{category.type}}</option>
                </select>
              </div>
               <div class="a-spacing-top-medium">
                <label>Owner</label>
                <select class="a-select-option" v-model="ownerID">
                  <option v-for="owner in owners" :value="owner._id" :key="owner._id">{{owner.name}}</option>
                </select>
              </div>
              <div class="a-spacing-top-medium">
                <label>Title</label>
                <input type="text" v-model="title" :placeholder="product.title" class="a-input-text" style="width:100%;"/>
              </div>
              <div class="a-spacing-top-medium">
                <label>Price</label>
                <input type="number" v-model="price" :placeholder="product.price" class="a-input-text" style="width:100%;"/>
              </div>
               <div class="a-spacing-top-medium">
                <label>Stck Quantity</label>
                <input type="number"  v-model="stockQuantity" :placeholder="product.stockQuantity" class="a-input-text" style="width:100%;"/>
              </div>
              <div class="a-spacing-top-medium">
                <label>Description</label>
                <textarea v-model="description"  :placeholder="product.description" style="width:100%"/>
              </div>
              <div class="a-spacing-top-medium">
                <label>Add Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" @change="onFileSelected"/>
                    <p style="margin-top:-70px">{{fileName}}</p>
                  </label>
                </div>
              </div>
              <hr/> 
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onUpdateProduct">Add Product</span>
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
   async asyncData({$axios, params}){
    try {
      let categories = $axios.$get(`http://localhost:3000/api/categories`)
      let owners = $axios.$get(`http://localhost:3000/api/owners`)
      let product = $axios.$get(`http://localhost:3000/api/products/${params.id}`)

      const [catResponse, ownerResponse, productResponse] = await Promise.all([
        categories,
        owners,
        product
      ])

      return{
        categories: catResponse.categories,
        owners: ownerResponse.owners,
        product:productResponse.product

      }
    } catch (error) {
      console.log(error)
    }
   },
  data(){
    return{
      categoryID: null,
      ownerID: null,
      title:"",
      price:"",
      description:"",
      selectedFile:null,
      stockQuantity: "",
      fileName:null
    }
  },
  methods:{
    onFileSelected(event){
      this.selectedFile = event.target.files[0]
      this.fileName = event.target.files[0].name
    },
    async onUpdateProduct(){
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append("description", this.description);
      data.append("stockQuantity", this.stockQuantity);
      data.append("ownerID", this.ownerID);
      data.append("categoryID", this.categoryID);
      data.append("photo", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$put(`http://localhost:3000/api/products/${this.$route.params.id}`, data)
      this.$router.push("/")
    }
  }
}
</script>