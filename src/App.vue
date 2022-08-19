<template>
  <div id="app" class="container">
  
  <center><div id = "logo">
    <img src="./assets/2aCTI.png">
  </div></center>


  <h1 class="title mt-0">Report Classification</h1>
  
  <h2 class="mx-6">Enter the name of a report to predict the tactics and techniques (TTPs)</h2>
  <br><br>

  <div class="searchBar">
    <b-input placeholder="Search..." rounded v-model="searchBar"></b-input>
  </div>
  <br>
  <b-button type="is-link is-light" rounded @click="submitSearchForm">Search !</b-button><br />

  <br>
  <h2>or enter directly a report to retrieve its corresponding TTPs</h2>
  <br>

  <div class="textbox">
    <b-field>
      <b-input type="textarea"
        minlength="10"
        placeholder="Submit Report"
        v-model="searchTextArea">
      </b-input> 
    </b-field>
    <br>
  </div>

  <b-button type="is-link is-light" rounded @click="submitTextForm">Predict !</b-button><br />
  <br>

<b-loading :is-full-page="true" v-model="isLoading" :can-cancel="true"></b-loading>

<!-- Affichage search bar -->
<b-notification v-if="search_display_boxes"
  type="is-warning is-light"
  aria-close-label="Close notification"
  role="alert">
  {{this.description}}
</b-notification>

<!-- Affichage input text -->
<div class="columns" v-if="display_boxes || search_display_boxes">

  <div class="column">
        <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[0].label}}
          {{searchProbabilityValue(d_tactic[0].id)}}
        </p>
        <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[0].description}}
            </p>
        </b-collapse>
       

        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[0].id), 'has-background-danger': !tactics.includes(d_tactic[0].id) }">
        <div class="content" >
          <b-message v-for="technique in d_tactic[0].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
        </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[0].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>
    </div>
  </div>
<!-- Box 1 -->
  <div class="column">
    <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[1].label}}
          {{searchProbabilityValue(d_tactic[1].id)}}
        </p>

        <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[1].description}}
            </p>
        </b-collapse>

        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>

      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[1].id), 'has-background-danger': !tactics.includes(d_tactic[1].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[1].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[1].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>


    </div>
  </div>
  <!-- Box 2 -->
  <div class="column">
    <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[2].label}}
          {{searchProbabilityValue(d_tactic[2].id)}}
        </p>

        <b-collapse 
          :open="false" 
          position="is-bottom" 
          aria-id="contentIdForA11y4">
          <template #trigger="props">
              <a
                  aria-controls="contentIdForA11y4"
                  :aria-expanded="props.open">
                  <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                  {{ !props.open ? 'Show description' : 'Hide' }}
              </a>
          </template>
          <p>
            {{d_tactic[2].description}}
          </p>
        </b-collapse>
    

        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>

      <div  v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[2].id), 'has-background-danger': !tactics.includes(d_tactic[2].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[2].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
              :open="false" 
              position="is-bottom" 
              aria-id="contentIdForA11y4">
              <template #trigger="props">
                  <a
                      aria-controls="contentIdForA11y4"
                      :aria-expanded="props.open">
                      <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                      {{ !props.open ? 'Show description' : 'Hide' }}
                  </a>
              </template>
              <p>
                {{technique.description}}
              </p>
            </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[2].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div>
<!-- Box 3 -->
  <div class="column">
    <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[3].label}}
          {{searchProbabilityValue(d_tactic[3].id)}}
        </p>
        
        <b-collapse 
          :open="false" 
          position="is-bottom" 
          aria-id="contentIdForA11y4">
          <template #trigger="props">
              <a
                  aria-controls="contentIdForA11y4"
                  :aria-expanded="props.open">
                  <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                  {{ !props.open ? 'Show description' : 'Hide' }}
              </a>
          </template>
          <p>
              {{d_tactic[3].description}}
          </p>
        </b-collapse>
        

        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div  v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[3].id), 'has-background-danger': !tactics.includes(d_tactic[3].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[3].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[3].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>
    </div>
  </div>
  <!-- Box 4 -->
   <div class="column">
      <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[4].label}}
          {{searchProbabilityValue(d_tactic[4].id)}}
        </p>

        <b-collapse 
          :open="false" 
          position="is-bottom" 
          aria-id="contentIdForA11y4">
          <template #trigger="props">
              <a
                  aria-controls="contentIdForA11y4"
                  :aria-expanded="props.open">
                  <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                  {{ !props.open ? 'Show description' : 'Hide' }}
              </a>
          </template>
          <p>
              {{d_tactic[4].description}}
          </p>
        </b-collapse>
        

        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[4].id), 'has-background-danger': !tactics.includes(d_tactic[4].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[4].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
              :open="false" 
              position="is-bottom" 
              aria-id="contentIdForA11y4">
              <template #trigger="props">
                  <a
                      aria-controls="contentIdForA11y4"
                      :aria-expanded="props.open">
                      <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                      {{ !props.open ? 'Show description' : 'Hide' }}
                  </a>
              </template>
              <p>
                {{technique.description}}
              </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[4].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>
    </div>
  </div>
</div>
<!-- Box 5 -->
<div class="columns" v-if="display_boxes || search_display_boxes">
  <div class="column">
        <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[5].label}}
          {{searchProbabilityValue(d_tactic[5].id)}}
        </p>

          <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[5].description}}
            </p>
        </b-collapse>

        
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div  v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[5].id), 'has-background-danger': !tactics.includes(d_tactic[5].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[5].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[5].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div>
<!-- Box 6 -->
  <div class="column">
        <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[6].label}}
          {{searchProbabilityValue(d_tactic[6].id)}}
        </p>      
          <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[6].description}}
            </p>
        </b-collapse>

        
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[6].id), 'has-background-danger': !tactics.includes(d_tactic[6].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[6].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[6].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div>

<!-- Box 7 -->
   <div class="column">
        <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[7].label}}
          {{searchProbabilityValue(d_tactic[7].id)}}
        </p>

        <b-collapse 
          :open="false" 
          position="is-bottom" 
          aria-id="contentIdForA11y4">
          <template #trigger="props">
              <a
                  aria-controls="contentIdForA11y4"
                  :aria-expanded="props.open">
                  <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                  {{ !props.open ? 'Show description' : 'Hide' }}
              </a>
          </template>
          <p>
              {{d_tactic[7].description}}
          </p>
        </b-collapse>

        
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[7].id), 'has-background-danger': !tactics.includes(d_tactic[7].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[7].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message> 
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[8].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div>

  <!-- Box 8 -->
  <div class="column">
        <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[8].label}}
          {{searchProbabilityValue(d_tactic[8].id)}}
        </p>

          <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[8].description}}
            </p>
        </b-collapse>

        
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[8].id), 'has-background-danger': !tactics.includes(d_tactic[8].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[8].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message> 
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[8].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div>
</div>
<!-- Box 9 -->
<div class="columns" v-if="display_boxes">
  <div class="column">
    <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[9].label}}
          {{searchProbabilityValue(d_tactic[9].id)}}
        </p>

          <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[9].description}}
            </p>
        </b-collapse>

        
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[9].id), 'has-background-danger': !tactics.includes(d_tactic[9].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[9].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id)}" >
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[9].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div>
  <!-- Box 10 -->
  <div class="column">
        <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[10].label}}
          {{searchProbabilityValue(d_tactic[10].id)}}
        </p>

          <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[10].description}}
            </p>
        </b-collapse>

        
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[10].id), 'has-background-danger': !tactics.includes(d_tactic[10].id) }"> 
        <div class="content">
          <b-message v-for="technique in d_tactic[10].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[10].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div>

  <!-- Box 11 -->
  <div class="column">
    <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[11].label}}
          {{searchProbabilityValue(d_tactic[11].id)}}
         </p>

          <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{d_tactic[11].description}}
            </p>
          </b-collapse>
       
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[11].id), 'has-background-danger': !tactics.includes(d_tactic[11].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[11].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
            :open="false" 
            position="is-bottom" 
            aria-id="contentIdForA11y4">
            <template #trigger="props">
                <a
                    aria-controls="contentIdForA11y4"
                    :aria-expanded="props.open">
                    <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                    {{ !props.open ? 'Show description' : 'Hide' }}
                </a>
            </template>
            <p>
               {{technique.description}}
            </p>
          </b-collapse>
          </b-message>
        </div>
      </div>

      <div v-if="search_display_boxes" class="card-content">
        <div class="content">
          <b-message v-for="technique in d_tactic[11].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
            {{technique.label}}
            <b-collapse 
                :open="false" 
                position="is-bottom" 
                aria-id="contentIdForA11y4">
                <template #trigger="props">
                    <a
                        aria-controls="contentIdForA11y4"
                        :aria-expanded="props.open">
                        <b-icon :icon="!props.open ? 'menu-down' : 'menu-up'"></b-icon>
                        {{ !props.open ? 'Show description' : 'Hide' }}
                    </a>
                </template>
                <p>
                  {{technique.description}}
                </p>
            </b-collapse>
          </b-message> 
        </div>
      </div>

    </div>
  </div> 
</div>

  <div class="sent" v-if="display_boxes">
    <b-message 
      title="Relevant Sentences:" 
      type="is-info" 
      has-icon 
      aria-close-label="Relevant Sentences: ">
      <p v-for="(sentence, index) in sentences" :key="index" class="my-4">
        {{sentence}}
      </p>
    </b-message>
  </div>
</div>


</template>

<script>
import axios from 'axios'; // chercher dans node module axios

export default {
  name: 'App',
  data() {
    return {
      d_tactic: [
        {
          id: 'TA0043',
          label: '1) Reconnaissance (TA0043)',
          description: "The adversary is trying to establish resources they can use to support operations.",
          d_techniques: [
            {
              id: 'T1595', 
              label: 'Active Scanning',
              description: 'Adversaries may execute active reconnaissance scans to gather information that can be used during targeting. Active scans are those where the adversary probes victim infrastructure via network traffic, as opposed to other forms of reconnaissance that do not involve direct interaction.'
            },
            {
              id: 'T1592',
              label: 'Gather Victim Host Information',
              description: 'Adversaries may gather information about the victim hosts that can be used during targeting. Information about hosts may include a variety of details, including administrative data (ex: name, assigned IP, functionality, etc.) as well as specifics regarding its configuration (ex: operating system, language, etc.).'
            },
            {
              id: 'T1589',
              label: 'Gather Victim Identity Information'
            },
            {
              id: 'T1590', 
              label: 'Gather Victim Network Information'
            },
            {
              id: 'T1591',
              label: 'Gather Victim Org Information'
            },
            {
              id: 'T1598',
              label: 'Phishing for Information'
            },
            {
              id: 'T1597', 
              label: 'Search Closed Sources'
            },
            {
              id: 'T1596',
              label: 'Search Open Technical Databases'
            },
            {
              id: 'T1593',
              label: 'Search Open Websites/Domains'
            },
            {
              id: 'T1594',
              label: 'Search Victim-Owned Websites'
            },
          ]
        },
        {
          id: 'TA0042', 
          label: '2) Resource Development (TA0042)',
          description: "The adversary is trying to establish resources they can use to support operations.",
          d_techniques: [
            {
              id: 'T1583', 
              label: 'Acquire Infrastructure'
            },
            {
              id: 'T1586',
              label: 'Compromise Accounts'
            },
            {
              id: 'T1584',
              label: 'Compromise Infrastructure'
            },
            {
              id: 'T1587', 
              label: 'Develop Capabilities'
            },
            {
              id: 'T1585',
              label: 'Establish Accounts'
            },
            {
              id: 'T1588',
              label: 'Obtain Capabilities'
            },
            {
              id: 'T1608', 
              label: 'Stage Capabilities'
            }
          ]
        },
        {
          id: 'TA0001',
          label: '3) Initial Access (TA0001)',
          description: 'The adversary is trying to get into your network.',
          d_techniques: [
            {
              id: 'T1189', 
              label: 'Drive-by Compromise'
            },
            {
              id: 'T1190',
              label: 'Exploit Public-Facing Application'
            },
            {
              id: 'T1133',
              label: 'External Remote Services'
            },
            {
              id: 'T1200', 
              label: 'Hardware Additions'
            },
            {
              id: 'T1566',
              label: 'Phishing'
            },
            {
              id: 'T1091',
              label: 'Replication Through Removable Media'
            },
            {
              id: 'T1195', 
              label: 'Supply Chain Compromise'
            },
            {
              id: 'T1199',
              label: 'Trusted Relationship'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts'
            }
          ]
        },
        {
          id: 'TA0002',
          label: '4) Execution (TA0002)',
          d_techniques: [
            {
              id: 'T1059', 
              label: 'Command and Scripting Interpreter'
            },
            {
              id: 'T1609',
              label: 'Container Administration Command'
            },
            {
              id: 'T1610',
              label: 'Deploy Container'
            },
            {
              id: 'T1203', 
              label: 'Exploitation for Client Execution'
            },
            {
              id: 'T1559',
              label: 'Inter-Process Communication'
            },
            {
              id: 'T1106',
              label: 'Native API'
            },
            {
              id: 'T1053', 
              label: 'Scheduled Task/Job'
            },
            {
              id: 'T1129',
              label: 'Shared Modules'
            },
            {
              id: 'T1072', 
              label: 'Software Deployment Tools'
            },
            {
              id: 'T1569', 
              label: 'System Services'
            },
            {
              id: 'T1204',
              label: 'User Execution'
            },
            {
              id: 'T1047', 
              label: 'Windows Management Instrumentation'
            }
          ]
        },
        {
          id: 'TA0003', 
          label: '5) Persistence (TA0003)',
          d_techniques: [
            {
              id: 'T1098', 
              label: 'Account Manipulation'
            },
            {
              id: 'T1197',
              label: 'BITS Jobs'
            },
            {
              id: 'T1547',
              label: 'Boot or Logon Autostart Execution'
            },
            {
              id: 'T1037', 
              label: 'Boot or Logon Initialization Scripts'
            },
            {
              id: 'T1176',
              label: 'Browser Extensions'
            },
            {
              id: 'T1554',
              label: 'Compromise Client Software Binary'
            },
            {
              id: 'T1136', 
              label: 'Create Account'
            },
            {
              id: 'T1543',
              label: 'Create or Modify System Process'
            },
            {
              id: 'T1546', 
              label: 'Event Triggered Execution'
            },
            {
              id: 'T1133', 
              label: 'External Remote Services'
            },
            {
              id: 'T1574',
              label: 'Hijack Execution Flow'
            },
            {
              id: 'T1525', 
              label: 'Implant Internal Image'
            },
            {
              id: 'T1556',
              label: 'Modify Authentication Process'
            },
            {
              id: 'T1137', 
              label: 'Office Application Startup'
            },
            {
              id: 'T1542',
              label: 'Pre-OS Boot'
            },
            {
              id: 'T1053', 
              label: 'Scheduled Task/Job'
            },
            {
              id: 'T1505', 
              label: 'Server Software Component'
            },
            {
              id: 'T1205',
              label: 'Traffic Signaling'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts'
            }
          ]
        },
        {
          id: 'TA0004', 
          label: '6) Privilege Escalation (TA0004)',
          d_techniques: [
            {
              id: 'T1548', 
              label: 'Abuse Elevation Control Mechanism'
            },
            {
              id: 'T1134',
              label: 'Access Token Manipulation'
            },
            {
              id: 'T1547',
              label: 'Boot or Logon Autostart Execution'
            },
            {
              id: 'T1037', 
              label: 'Boot or Logon Initialization Scripts'
            },
            {
              id: 'T1543',
              label: 'Create or Modify System Process'
            },
            {
              id: 'T1484',
              label: 'Domain Policy Modification'
            },
            {
              id: 'T1611', 
              label: 'Escape to Host'
            },
            {
              id: 'T1546',
              label: 'Event Triggered Execution'
            },
            {
              id: 'T1068', 
              label: 'Exploitation for Privilege Escalation'
            },
            {
              id: 'T1055', 
              label: 'Process Injection'
            },
            {
              id: 'T1053',
              label: 'Scheduled Task/Job'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts'
            }
          ]
        },
        {
          id: 'TA0005', 
          label: '7) Defense Evasion (TA0005)',
          d_techniques: [
            {
              id: 'T1548', 
              label: 'Abuse Elevation Control Mechanism'
            },
            {
              id: 'T1134',
              label: 'Access Token Manipulation'
            },
            {
              id: 'T1197',
              label: 'BITS Jobs'
            },
            {
              id: 'T1612', 
              label: 'Build Image on Host'
            },
            {
              id: 'T1622',
              label: 'Debugger Evasion'
            },
            {
              id: 'T1140',
              label: 'Deobfuscate/Decode Files or Information'
            },
            {
              id: 'T1610', 
              label: 'Deploy Container'
            },
            {
              id: 'T1006',
              label: 'Direct Volume Access'
            },
            {
              id: 'T1484', 
              label: 'Domain Policy Modification'
            },
            {
              id: 'T1480', 
              label: 'Execution Guardrails'
            },
            {
              id: 'T1211',
              label: 'Exploitation for Defense Evasion'
            },
            {
              id: 'T1222', 
              label: 'File and Directory Permissions Modification'
            },
            {
              id: 'T1564', 
              label: 'Hide Artifacts'
            },
            {
              id: 'T1574', 
              label: 'Hijack Execution Flow'
            },
            {
              id: 'T1562',
              label: 'Impair Defenses'
            },
            {
              id: 'T1070', 
              label: 'Indicator Removal on Host'
            },
            {
              id: 'T1202',
              label: 'Indirect Command Execution'
            },
            {
              id: 'T1036', 
              label: 'Masquerading'
            },
            {
              id: 'T1556',
              label: 'Modify Authentication Process'
            },
            {
              id: 'T1578', 
              label: 'Modify Cloud Compute Infrastructure'
            },
            {
              id: 'T1112', 
              label: 'Modify Registry'
            },
            {
              id: 'T1601',
              label: 'Modify System Image'
            },
            {
              id: 'T1599', 
              label: 'Network Boundary Bridging'
            },
            {
              id: 'T1027', 
              label: 'Obfuscated Files or Information'
            },
            {
              id: 'T1647', 
              label: 'Plist File Modification'
            },
            {
              id: 'T1542',
              label: 'Pre-OS Boot'
            },
            {
              id: 'T1055', 
              label: 'Process Injection'
            },
            {
              id: 'T1620', 
              label: 'Reflective Code Loading'
            },
            {
              id: 'T1207',
              label: 'Rogue Domain Controller'
            },
            {
              id: 'T1014', 
              label: 'Rootkit'
            },
            {
              id: 'T1553', 
              label: 'Subvert Trust Controls'
            },
            {
              id: 'T1218',
              label: 'System Binary Proxy Execution'
            },
            {
              id: 'T1216', 
              label: 'System Script Proxy Execution'
            },
            {
              id: 'T1221', 
              label: 'Template Injection'
            },
            {
              id: 'T1205', 
              label: 'Traffic Signaling'
            },
            {
              id: 'T1127',
              label: 'Trusted Developer Utilities Proxy Execution'
            },
            {
              id: 'T1535', 
              label: 'Unused/Unsupported Cloud Regions'
            },
            {
              id: 'T1550', 
              label: 'Use Alternate Authentication Material'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts'
            },
            {
              id: 'T1497',
              label: 'Virtualization/Sandbox Evasion'
            },
            {
              id: 'T1600', 
              label: 'Weaken Encryption'
            },
            {
              id: 'T1220', 
              label: 'XSL Script Processing'
            }
          ]
        },
        {
          id: 'TA0006',
          label: '8) Credential Access (TA0006)',
          d_techniques: [
            {
              id: 'T1557', 
              label: 'Adversary-in-the-Middle'
            },
            {
              id: 'T1110',
              label: 'Brute Force'
            },
            {
              id: 'T1555',
              label: 'Credentials from Password Stores'
            },
            {
              id: 'T1212', 
              label: 'Exploitation for Credential Access'
            },
            {
              id: 'T1187',
              label: 'Forced Authentication'
            },
            {
              id: 'T1606',
              label: 'Forge Web Credentials'
            },
            {
              id: 'T1056', 
              label: 'Input Capture'
            },
            {
              id: 'T1556',
              label: 'Modify Authentication Process'
            },
            {
              id: 'T1111', 
              label: 'Multi-Factor Authentication Interception'
            },
            {
              id: 'T1621', 
              label: 'Multi-Factor Authentication Request Generation'
            },
            {
              id: 'T1040',
              label: 'Network Sniffing'
            },
            {
              id: 'T1003', 
              label: 'OS Credential Dumping'
            },
            {
              id: 'T1528', 
              label: 'Steal Application Access Token'
            },
            {
              id: 'T1558', 
              label: 'Steal or Forge Kerberos Tickets'
            },
            {
              id: 'T1539',
              label: 'Steal Web Session Cookie'
            },
            {
              id: 'T1552', 
              label: 'Unsecured Credentials'
            }
          ]
        },
        {
          id: 'TA0007',
          label: '9) Discovery (TA0007)',
          d_techniques: [
            {
              id: 'T1046', 
              label: 'Network Service Discovery'
            },
            {
              id: 'T1087', 
              label: 'Account Discovery'
            },
            {
              id: 'T1010',
              label: 'Application Window Discovery'
            },
            {
              id: 'T1217',
              label: 'Browser Bookmark Discovery'
            },
            {
              id: 'T1580', 
              label: 'Cloud Infrastructure Discovery'
            },
            {
              id: 'T1538',
              label: 'Cloud Service Dashboard'
            },
            {
              id: 'T1526',
              label: 'Cloud Service Discovery'
            },
            {
              id: 'T1619', 
              label: 'Cloud Storage Object Discovery'
            },
            {
              id: 'T1613',
              label: 'Container and Resource Discovery'
            },
            {
              id: 'T1622', 
              label: 'Debugger Evasion'
            },
            {
              id: 'T1482', 
              label: 'Domain Trust Discovery'
            },
            {
              id: 'T1083',
              label: 'File and Directory Discovery'
            },
            {
              id: 'T1615', 
              label: 'Group Policy Discovery'
            },
            {
              id: 'T1135', 
              label: 'Network Share Discovery'
            },
            {
              id: 'T1201', 
              label: 'Password Policy Discovery'
            },
            {
              id: 'T1120',
              label: 'Peripheral Device Discovery'
            },
            {
              id: 'T1069', 
              label: 'Permission Groups Discovery'
            },
            {
              id: 'T1057', 
              label: 'Process Discovery'
            },
            {
              id: 'T1012',
              label: 'Query Registry'
            },
            {
              id: 'T1018', 
              label: 'Remote System Discovery'
            },
            {
              id: 'T1518', 
              label: 'Software Discovery'
            },
            {
              id: 'T1082', 
              label: 'System Information Discovery'
            },
            {
              id: 'T1614',
              label: 'System Location Discovery'
            },
            {
              id: 'T1016', 
              label: 'System Network Configuration Discovery'
            },
            {
              id: 'T1049',
              label: 'System Network Connections Discovery'
            },
            {
              id: 'T1033', 
              label: 'System Owner/User Discovery'
            },
            {
              id: 'T1007', 
              label: 'System Service Discovery'
            },
            {
              id: 'T1124', 
              label: 'System Time Discovery'
            },
            {
              id: 'T1497',
              label: 'Virtualization/Sandbox Evasion'
            }
          ]
        },
        {
          id: 'TA0008', 
          label: '10) Lateral Movement (TA0008)',
          d_techniques: [
            {
              id: 'T1210', 
              label: 'Exploitation of Remote Services'
            },
            {
              id: 'T1534',
              label: 'Internal Spearphishing'
            },
            {
              id: 'T1570',
              label: 'Lateral Tool Transfer'
            },
            {
              id: 'T1563', 
              label: 'Remote Service Session Hijacking'
            },
            {
              id: 'T1021',
              label: 'Remote Services'
            },
            {
              id: 'T1091',
              label: 'Replication Through Removable Media'
            },
            {
              id: 'T1072', 
              label: 'Software Deployment Tools'
            },
            {
              id: 'T1080',
              label: 'Taint Shared Content'
            },
            {
              id: 'T1550', 
              label: 'Use Alternate Authentication Material'
            }
          ]
        },
        {
          id: 'TA0009',
          label: '11) Collection (TA0009)', 
          d_techniques: [
            {
              id: 'T1557', 
              label: 'Adversary-in-the-Middle'
            },
            {
              id: 'T1560',
              label: 'Archive Collected Data'
            },
            {
              id: 'T1123',
              label: 'Audio Capture'
            },
            {
              id: 'T1119', 
              label: 'Automated Collection'
            },
            {
              id: 'T1185',
              label: 'Browser Session Hijacking'
            },
            {
              id: 'T1115',
              label: 'Clipboard Data'
            },
            {
              id: 'T1530', 
              label: 'Data from Cloud Storage Object'
            },
            {
              id: 'T1602',
              label: 'Data from Configuration Repository'
            },
            {
              id: 'T1213', 
              label: 'Data from Information Repositories'
            },
            {
              id: 'T1005',
              label: 'Data from Local System'
            },
            {
              id: 'T1039',
              label: 'Data from Network Shared Drive'
            },
            {
              id: 'T1025', 
              label: 'Data from Removable Media'
            },
            {
              id: 'T1074',
              label: 'Data Staged'
            },
            {
              id: 'T1114', 
              label: 'Email Collection'
            },
            {
              id: 'T1056',
              label: 'Input Capture'
            },
            {
              id: 'T1113',
              label: 'Screen Capture'
            },
            {
              id: 'T1125', 
              label: 'Video Capture'
            }
          ]
        },
        {
          id: 'TA0011',
          label: '12) Command and Control (TA0011)',
          d_techniques: [
            {
              id: 'T1071', 
              label: 'Application Layer Protocol'
            },
            {
              id: 'T1092',
              label: 'Communication Through Removable Media'
            },
            {
              id: 'T1132',
              label: 'Data Encoding'
            },
            {
              id: 'T1001', 
              label: 'Data Obfuscation'
            },
            {
              id: 'T1568',
              label: 'Dynamic Resolution'
            },
            {
              id: 'T1573',
              label: 'Encrypted Channel'
            },
            {
              id: 'T1008', 
              label: 'Fallback Channels'
            },
            {
              id: 'T1105',
              label: 'Ingress Tool Transfer'
            },
            {
              id: 'T1104', 
              label: 'Multi-Stage Channels'
            },
            {
              id: 'T1095',
              label: 'Non-Application Layer Protocol'
            },
            {
              id: 'T1571',
              label: 'Non-Standard Port'
            },
            {
              id: 'T1572', 
              label: 'Protocol Tunnelin'
            },
            {
              id: 'T1090',
              label: 'Proxy'
            },
            {
              id: 'T1219', 
              label: 'Remote Access Software'
            },
            {
              id: 'T1205',
              label: 'Traffic Signaling'
            },
            {
              id: 'T1102',
              label: 'Web Service'
            }
          ]
        },
        {
          id: 'TA0010', 
          label: '13) Exfiltration (TA0010)',
          d_techniques: [
            {
              id: 'T1020', 
              label: 'Automated Exfiltration'
            },
            {
              id: 'T1030',
              label: 'Data Transfer Size Limits'
            },
            {
              id: 'T1048',
              label: 'Exfiltration Over Alternative Protocol'
            },
            {
              id: 'T1041', 
              label: 'Exfiltration Over C2 Channel'
            },
            {
              id: 'T1011',
              label: 'Exfiltration Over Other Network Medium'
            },
            {
              id: 'T1052',
              label: 'Exfiltration Over Physical Medium'
            },
            {
              id: 'T1567', 
              label: 'Exfiltration Over Web Service'
            },
            {
              id: 'T1029',
              label: 'Scheduled Transfer'
            },
            {
              id: 'T1537', 
              label: 'Transfer Data to Cloud Account'
            }
          ]
        },
        {
          id: 'TA0040',
          label: '14) Impact',
          d_techniques: [
            {
              id: 'T1531', 
              label: 'Account Access Removal'
            },
            {
              id: 'T1485',
              label: 'Data Destruction'
            },
            {
              id: 'T1486',
              label: 'Data Encrypted for Impact'
            },
            {
              id: 'T1565', 
              label: 'Data Manipulation'
            },
            {
              id: 'T1491',
              label: 'Defacement'
            },
            {
              id: 'T1561',
              label: 'Disk Wipe'
            },
            {
              id: 'T1499', 
              label: 'Endpoint Denial of Service'
            },
            {
              id: 'T1495',
              label: 'Firmware Corruption'
            },
            {
              id: 'T1490', 
              label: 'Inhibit System Recovery'
            },
            {
              id: 'T1498',
              label: 'Network Denial of Service'
            },
            {
              id: 'T1496',
              label: 'Resource Hijacking'
            },
            {
              id: 'T1489', 
              label: 'Service Stop'
            },
            {
              id: 'T1529',
              label: 'System Shutdown/Reboot'
            }
          ]
        }
      ],
      tactics: [],
      techniques: [],
      description: "",
      probabilities: [],
      sentences: [],
      searchBar: '',
      searchTextArea: '', 
      isLoading: false, 
      api_url: "http://localhost:8000/classification", // change api address 
      search_url: "http://localhost:8000/searchattack",
      display_boxes: false,
      search_display_boxes: false,
    }
  },
  methods: {
    async submitSearchForm() {
      this.search_display_boxes = false
      this.display_boxes = false
      this.isLoading = true
      let response = await axios.post(this.search_url, {'sentence': this.searchBar}) // call API
      .then(response => {
        this.techniques = response.data.techniques
        this.description = response.data.description
        }) // retrieve response
      this.isLoading = false
      this.search_display_boxes = true
      this.display_boxes = false
    },
    async submitTextForm(){ // asynchronous request
      this.display_boxes = false
      this.search_display_boxes = false
      this.isLoading = true
      let response = await axios.post(this.api_url, {'sentence': this.searchTextArea}) // call API
      .then(response => {
        console.log(response)
        this.tactics = response.data.tactics
        this.techniques = response.data.techniques
        this.sentences = response.data.relevant_sents
        for (let [key, value] of Object.entries(response.data.relevant_tactic_dict)) {
          console.log(`${key}: ${value}`);
          this.probabilities.push(key);
          this.probabilities.push(value);
        }
      }) // retrieve response
      this.isLoading = false
      this.display_boxes = true
      this.search_display_boxes = false
    },
    searchProbabilityValue(tacticID){
      console.log(this.probabilities)
      const isSameValue = (element) => element == tacticID
      const keyIndex = this.probabilities.findIndex(isSameValue);
      console.log(keyIndex)
      if(keyIndex != -1){
        return this.probabilities[keyIndex + 1];
      }
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color:midnightblue;
}
.searchBar{
  width: 600px;
  margin: 0 auto;
}
.textbox{
  width: 600px;
  margin: 0 auto;
}
h1.title{
  font-size: 35px;
  color: rgb(27, 27, 125);
  margin-top: 0%; 
}
h2{
  color: #666e77;
  font-size: 20px;
}
#nav {
  padding: 30px;
}
input{
    display: inline; 
    padding: 10px 6px; 
    margin: 30px;
    width: 60%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd; 
    color: rgb(141, 138, 138); 
  }
button{
  margin: 0 10px; 
  padding: 10px; 
  border: none; 
  border-radius: 4px; 
  color: midnightblue;
}
#logo {
  height: 200px;
  width: 310px; 
  text-align: center;
  display: flex;
}
#logo > img{
  text-align: center;
  align-content: center;
  max-width: 100%;
  max-height: 100%;
}
footer{
  display: inline;
  text-align: center;
  height: 50px;
}
</style>
