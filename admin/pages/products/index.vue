<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="text-align: center">Add a new Product</h2>
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
                <input type="text" v-model="title" class="a-input-text" style="width:100%;"/>
              </div>
              <div class="a-spacing-top-medium">
                <label>Price</label>
                <input type="number" v-model="price" class="a-input-text" style="width:100%;"/>
              </div>
               <div class="a-spacing-top-medium">
                <label>Stck Quantity</label>
                <input type="number"  v-model="stockQuantity" class="a-input-text" style="width:100%;"/>
              </div>
              <div class="a-spacing-top-medium">
                <label>Description</label>
                <textarea placeholder="provide a detail" v-model="description" style="width:100%"/>
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
                    <span class="a-button-text" @click="onAddProduct">Add Product</span>
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
  async asyncData({$axios}){
    try{
      let categories = $axios.$get("http://localhost:3000/api/categories")
      let owners = $axios.$get("http://localhost:3000/api/owners")
      const [catResponse, ownerResponse] = await Promise.all([categories, owners])
      console.log(catResponse);
      return{
        categories:catResponse.categories,
        owners:ownerResponse.owners
      }
    }catch(err){
      console.log(err)
    }
  },
  data(){
    return{
      categoryID: null,
      ownerID: null,
      title:"",
      price:0,
      description:"",
      selectedFile:null,
      stockQuantity: 1,
      fileName:null
    }
  },
  methods:{
    onFileSelected(event){
      this.selectedFile = event.target.files[0]
      this.fileName = event.target.files[0].name
    },
    async onAddProduct(){
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append("description", this.description);
      data.append("stockQuantity", this.stockQuantity);
      data.append("ownerID", this.ownerID);
      data.append("categoryID", this.categoryID);
      data.append("photo", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$post("http://localhost:3000/api/products", data)
      this.$router.push("/")
    }
  }
}
</script>