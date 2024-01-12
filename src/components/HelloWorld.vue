<script setup>
defineProps({
  msg: {
    type: String,
    required: true,
  },
});

const sendData = async () => {
  let response = await fetch('https://test-flask-app-tngh.onrender.com/post', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      name: document.getElementById('name').value,
      age: document.getElementById('age').value,
      phone_no: document.getElementById('phone').value,
    }),
  });

  if (response.ok) {
    window.alert('sent!');
  }

}

const getData = async () => {
  try {
    let response = await fetch('https://test-flask-app-tngh.onrender.com/get');

    if (!response.ok) {
      throw new Error('Network response was not ok');
    }

    let dataArray = await response.json();
    console.log(dataArray);

    // Clear existing content
    const container = document.getElementById('data-container');
    container.innerHTML = '';

    // Iterate over the array and create HTML elements for each object
    dataArray.forEach(data => {
      const div = document.createElement('div');

      const nameElement = document.createElement('span');
      nameElement.innerHTML = `Name: ${data.name}`;
      div.appendChild(nameElement);
      
      const ageElement = document.createElement('span');
      ageElement.innerHTML = `      Age: ${data.age}`;
      div.appendChild(ageElement);

      const phoneElement = document.createElement('span');
      phoneElement.innerHTML = `      Phone: ${data.phone_no}`;
      div.appendChild(phoneElement);

      container.appendChild(div);
    });

  } catch (error) {
    console.error('Error fetching data:', error.message);
  }

}

</script>

<template>
  <div class="greetings">
    <div class="form">
      <h1 class="green">{{ msg }}</h1>
      <label for="name">Name</label>
      <input type="text" name="name" id="name" class="text-box" /><br />

      <label for="age">Age</label>
      <input type="number" name="age" id="age" class="text-box" /><br>

      <label for="phone">Phone No</label>
      <input type="tel" name="phone" id="phone" class="text-box" /><br>

      <button @click="sendData">submit</button>
      <br>
      <button @click="getData">get</button>
      <div id="data-container">
        <!-- Data will be dynamically added here -->
      </div>
    </div>
  </div>
</template>

<style scoped>

.text-box{
  height: 25px;
}
.green {
  color: rgb(255, 255, 255);
}

.form {
  display: flex;
  flex-direction: column;
  align-content: center;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}


.greetings {
  text-align: center;
  display: flex;
  justify-content: space-between;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
