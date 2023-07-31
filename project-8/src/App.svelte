<script>
    import { onMount } from "svelte";
  
    let users = [];
    let id, name, email;
    let control = false;
    let userId;
    let jsonURL = "http://localhost:3000";
  
    async function addUser() {
      if (users.length > 0) {
        id = users[users.length - 1].id;
      } else {
        id = 0;
      }
      const obj = { id: id + 1, name, email };
      const response = await fetch(`${jsonURL}/users`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(obj),
      });
  
      if (response.ok) {
        getAll();
      } else {
        console.error("Kullanıcıyı sunucuya eklerken hata oluştu.");
      }
    }
  
    async function editUser() {
      const userToUpdate = users.find((data) => data.id === userId);
      if (userToUpdate) {
        userToUpdate.name = name;
        userToUpdate.email = email;
        const response = await fetch(`${jsonURL}/users/${userId}`, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(userToUpdate),
        });
  
        if (response.ok) {
          getAll();
        } else {
          console.error("Kullanıcıyı sunucuda güncellerken hata oluştu.");
        }
      }
    }
  
    async function getAll() {
      const response = await fetch(`${jsonURL}/users`);
  
      if (response.ok) {
        const data = await response.json();
        users = data;
      } else {
        console.error("Kullanıcıları sunucudan alırken hata oluştu.");
      }
      name = "";
      email = "";
      control = false;
    }
  
    async function deleteOne(id) {
      const response = await fetch(`${jsonURL}/users/${id}`, {
        method: "DELETE",
      });
  
      if (response.ok) {
        getAll();
      } else {
        console.error("Kullanıcıyı sunucudan silerken hata oluştu.");
      }
    }
  
    function getByID(id) {
      users.forEach((data) => {
        if (data.id === id) {
          name = data.name;
          email = data.email;
          control = true;
          userId = data.id;
        }
      });
    }
  
    onMount(async () => {
      await getAll();
    });
  </script>

<div class="grid-container">
   <div class="c-1">
    <label for="" class="form-label" style="font-size: 16px; text-align:center; font-weight: bold; color: #333;">İsim</label>
    <input class="form-control" bind:value={name} type="text" placeholder="İsim Girin" style="text-align:center; width: 250px; margin-left:190px; padding: 10px; border: 1px solid #ccc; border-radius: 5px; font-size: 14px; color: #555;">
    
    <label for="" class="form-label" style="text-align:center; font-size: 16px; font-weight: bold; color: #333;">Email</label>
    <input type="text" bind:value={email} placeholder="Email Girin" style="text-align:center; width: 250px; padding: 10px; margin-left:190px; border: 1px solid #ccc; border-radius: 5px; font-size: 14px; color: #555;">    
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
                <td><button class="editBtn"  on:click={()=>{getByID(user.id)}}>Düzenle</button></td>
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
    margin-left: 270px;
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