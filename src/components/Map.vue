<script setup>

</script>

<template>
    <div class="wrap input-container">
        <div class="search-box"><input type="text" name="search" id="search" placeholder="Поиск"></div>
        <div class="buildings-box">
            <input type="radio" name="building" value="territory" id="building-territory">
            <label for="building-territory">Территория</label>

            <input type="radio" name="building" value="k1" id="building-k1">
            <label for="building-k1">Корпус 1</label>

            <input type="radio" name="building" value="k2" id="building-k2">
            <label for="building-k2">Корпус 2</label>

            <input type="radio" name="building" value="k3" id="building-k3">
            <label for="building-k3">Корпус 3</label>
        </div>
        <div class="floors-box">
            <span>Этаж 1</span>
        </div>
    </div>
</template>

<style scoped lang="scss">
    .input-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 1.2rem .8rem;
        background-color: var(--color-box);
        color: var(--color-inactive-text);
    }

    .buildings-box {
    }

    input[type="radio"] {
        all: unset;
        display: none;
    }

    label {
        all: unset;
        padding: 0 .4rem;
    }

    input[type="radio"]:checked+label {
        font-weight: 600;
        color: var(--color-text);
    }
</style>