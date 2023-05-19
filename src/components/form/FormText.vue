<template>
    <h1>
      Entretien Muvraline
    </h1>
    <div class="container" @keydown="handleKeyDown" @keyup="handleKeyUp" tabindex="0">
      <form @submit="handleSubmit">
        <div id="text-options">
          <input type="text" name="input-text" placeholder="enter your text area" id="input-text">
  
          <select v-model="selectedItem">
            <option v-for="item in menuItems" :value="item">{{ item.label }}</option>
          </select>
        </div>

        <button type="submit"> Execute </button>
      </form>
      <div id="text-output">
        <p>
          {{ outputText }}
        </p>
      </div>
      <ul>
        <li> Alt + Enter : retire les espaces </li>
        <li> Shift + Enter : Transforme en lowercase </li>
        <li> Alt + Shift + Enter : Transforme en uppercase</li>
      </ul>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        selectedItem: null,
        menuItems: [
          { id: "rmspace", label: "Remove spaces" },
          { id: "lower", label: "lowercase" },
          { id: "upper", label: "UPPERCASE" },
          { id: "none", label: "Dispay Text"}
        ],
        outputText: "",
      }
    },
    
    methods: {

      handleSubmit(event) {
        const value = document.getElementById("input-text").value;
        event.preventDefault(); // Prevents the form from submitting and refreshing the page
        this.displayResult(value, this.selectedItem.id);
      },

      displayResult(value, option) {
        let result = "";
        switch (option) {
          case "rmspace":
            const rmspaceString = value.replace(/\s/g, "");
            result = rmspaceString;
            break;
          case "lower":
            const lowercaseString = value.toLowerCase();
            result = lowercaseString;
            break;
          case "upper":
            const uppercaseString = value.toUpperCase();
            result = uppercaseString;
            break;
          case "none":
            result = value;
            break;
        }
        this.outputText = result;
      },

      handleKeyDown(event) {
        const { altKey, metaKey, shiftKey, code } = event;
        
        /* switch (true) allow to evaluate multiple expressions within each case */
        switch (true) {
          case altKey && code === "Enter" && shiftKey === false:
            this.displayResult(this.outputText, "rmspace");
            break;
          case shiftKey && code === "Enter" && altKey === false:
            this.displayResult(this.outputText, "lower");
            break;
          case altKey && shiftKey && code === "Enter":
            this.displayResult(this.outputText, "upper");
            break;
          default:
            break;        
        }
    },

    handleKeyUp(event) {
      // Reset the state of 'Alt/Option' key if released
      if (event.code === "AltLeft" || event.code === "AltRight") {
        this.altKey = false;
      }

      // Reset the state of 'Shift' key if released
      if (event.code === "ShiftLeft" || event.code === "ShiftRight") {
        this.shiftKey = false;
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  height: 100%;
  width: 100%;
  
  ul, form {
    margin: 0 auto 0.75rem;
  }

  form {

    display: flex;
    flex-direction: column;
    gap: 0.75rem;

    #text-options {
      padding-top: 1vh;
      display: flex;
      justify-content: center;
      select {
        padding: 0.5rem;
        border: 1px solid #ccc;
        border-radius: 4px;
      }
    }

    button {
      padding: 0.5rem 1rem;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;

      &:hover {
        background-color: #0056b3;
      }     
    }
  }
  
  #text-output {
    min-width: 100%;
    min-height: 10vh;
    font-size: 24px;
    text-align: center;
  }
}
</style>