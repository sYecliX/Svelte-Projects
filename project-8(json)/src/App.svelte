<script>
  import { onMount } from "svelte";

   let users = [{ name:"Veriler Yükleniyor ...",email:""}]
   let id, name,email
   let control = false
   let userId

   function addUser(){
    users.forEach((x)=>{
        id = x.id
    })
    let obj = {id:id+1,name, email}
    users.push(obj)
    getAll()
   }

   function editUser(id){
   users.forEach((data)=>{
    if(data.id == id){
       data.name = name
       data.email = email
    }
   })
   getAll()
   }
   function getAll(){
    users = users
    name = ''
    email = ''
    control = false
   }

   function deleteOne(id){
    users =  users.filter((x)=>x.id != id)
   }

   function edit(id){
    users.forEach((data)=>{
        if(data.id == id){
            name = data.name;
            email = data.email;
            control = true
            userId = data.id
        }
    })
    
   }
   onMount(async ()=>{
    let apiURL = 'https://jsonplaceholder.typicode.com/users'
    let response = await fetch(apiURL);
     users = await response.json();
   })
</script>


<div class="grid-container">
   <div class="c-1">
    <label for="" class="form-label" style="font-size: 16px; font-weight: bold; color: #333;">İsim</label>
    <input class="form-control" bind:value={name} type="text" placeholder="İsim Girin" style="width: 250px; padding: 10px; border: 1px solid #ccc; border-radius: 5px; font-size: 14px; color: #555;">
    
    <label for="" class="form-label" style="font-size: 16px; font-weight: bold; color: #333;">Email</label>
    <input type="text" bind:value={email} placeholder="Email Girin" style="width: 250px; padding: 10px; border: 1px solid #ccc; border-radius: 5px; font-size: 14px; color: #555;">    
        <div>
            <button class="save" on:click={()=>{
               control ?  editUser(userId) : addUser()
            }}>Kaydet</button>
        </div>
    </div>
    <div class="c-2">
        <table>
            <tr>
                <th>NAME</th>
                <th>E-MAIL</th>
            </tr>
            {#each users as user}
            <tr class="trStyle">
                <td>{user.name}</td>
                <td>{user.email}</td>
                <td><button class="deleteBtn" on:click={()=>{deleteOne(user.id)}}>Sil</button></td>
                <td><button class="editBtn"  on:click={()=>{edit(user.id)}}>Düzenle</button></td>
            </tr>
            {/each}        
        </table>
    </div>
</div>

<style>
.grid-container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    height: 100vh; 
}
.c-1, .c-2 {
    padding: 20px;
    border: 1px solid #ccc;
}
td{
    margin-left: 20px;
    margin-right: 20px;
    padding: 4px;
}
button{
    padding-left: 20px;
    padding-right: 20px;
    border: none;
    border-radius: 8px;
    color: white;
    box-shadow: 0 0 10px 4px rgba(102, 102, 102, 0.26);
} 
.save{
    background-color: green;
}
.save:hover{
    text-decoration: underline;
    cursor: pointer;
    background-color: rgba(26, 125, 9, 0.79);
    box-shadow: 0 0 10px 4px rgba(102, 102, 102, 0.548);
    transition: all ease 0.2s;
}
.deleteBtn{
    background-color: red;
}
.deleteBtn:hover{
    text-decoration: underline;
    cursor: pointer;
    background-color: rgb(251, 0, 0);
    box-shadow: 0 0 10px 4px rgba(102, 102, 102, 0.548);
    transition: all ease 0.2s;
}
.editBtn{
    background-color: rgb(255, 213, 0);
    box-shadow: 0 0 10px 4px rgba(102, 102, 102, 0.26);
}
.editBtn:hover{
    text-decoration: underline;
    cursor: pointer;
    background-color: rgb(255, 215, 15);
    box-shadow: 0 0 10px 4px rgba(102, 102, 102, 0.548);
    transition: all ease 0.2s;
}

.trStyle{
    background-color: rgb(75, 69, 69);
    color: white;
    text-align: center;
}
th{
    color: white;
    text-align: center;
    font-size: 17px;
    font-weight: 500;
    border: 6px solid rgb(75, 69, 69);
    background-color: rgb(75, 69, 69);
    margin-bottom: 5px;
}
</style>