<script>
    export default {
        props: {
            issues: {
                type: Array,
                default: () => []
            }
        },
        data(){
            return {
                isRotated: {}
            }
            
        },
        methods: {
            toggleRotation(issueId) {
                this.isRotated[issueId] = !this.isRotated[issueId]
            },
            resetRotation(issueId) {
                this.isRotated[issueId] = false
            }
        }
    }
</script>

<template>
    <div class="resultsContainer">
        <TransitionGroup name="results">
            <div class="resultItem" v-for="issue in issues" :key="issue.id" :class="{ 'rotateToggle': isRotated[issue.id], 'rotateToggleOff': !isRotated[issue.id] }" @mouseleave="resetRotation(issue.id)" @dragend="resetRotation(issue.id)">
                <a :href="issue.htmlUrl" target="_blank">{{ issue.gameName }}</a>
                <div class="gameId">{{ issue.gameId }}</div>
                <div class="status" @click="toggleRotation(issue.id);statusClicked = true" :class="{'boots': issue.status === 'boots', 'ingame': issue.status === 'ingame', 'menus': issue.status === 'menus', 'nothing': issue.status === 'nothing', 'playable': issue.status === 'playable', 'statusClicked': statusClicked }">{{ issue.status }}</div>
                <div class="resultItemBack">
                    <div class="insidePadding">
                        <h2>Labels:</h2>
                        <p>{{ issue.labels }}</p>
                    </div>
                </div>
            </div>
        </TransitionGroup>
    </div>
</template>

<style>
.rotateToggle {
    transform: rotateY(180deg);
    transform-style: preserve-3d;
    transition: 0.1s
}

.rotateToggleOff{
    transition: 0.1s
}

.resultItemBack {
    margin: 0;
    padding: 0;
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    top: 0;
    left: 0;
    background-color: #1e1e1e;
    transform: rotateY(180deg);
    border-radius: 5px;
}

.resultItemBack .insidePadding{
    margin: 0;
    padding: 5px;
}

.results-enter-active,
.results-leave-active {
    transition: all 0.5s ease;
}
.results-enter-from,
.results-leave-to {
    opacity: 0;
    font-size: 0;
    width: 0 !important;
    height: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
    transform: translateY(30px);
}

a {
    text-decoration: none;
    color: white;
    font-weight: bold;
}

.resultsContainer {	
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: center;
    transform-style: preserve-3d;
}

.resultItem {
    margin: 5px;
    padding: 5px;
    background-color: #1e1e1e;
    width: 180px;
    height: 228px;
    color: white;
    position: relative;
    border-radius: 5px;
    box-shadow: 0 0 3px black;
    transform-style: preserve-3d;
}

.boots {
    background-color: #c2e358;
    color: black;
}

.ingame {
    background-color: #2c9e4b;
}

.menus {
    background-color: #9ed763;
    color: black;
}

.nothing {
    background-color: #af1f2b;
}

.playable {
    background-color: #217638;
}

.status {
    /* position bottom right inside div */
    height: 100%;
    max-height: 20px;
    position: absolute;
    bottom: 0;
    right: 0;
    margin: 0;
    padding: 5px;
    border-radius: 10px 0 5px 0;
    transition: all 0.1s ease;
}

.statusClicked {
    max-height: 20px;
}

.status:hover {
    max-height: 30px;
    cursor: pointer;
}

.gameId {
    opacity: 0.5;
    font-family: 'Courier New', Courier, monospace;
    /* shadow */
    text-shadow: 1px 1px 1px black;
}
</style>