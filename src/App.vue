<template>
  <div id="app" class="container">
  
  <center><div id = "logo">
    <img src="./assets/2aCTI.png">
  </div></center>


  <h1 class="title mt-0">Automated Analysis of Cyber Threat Intelligence</h1>
  
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
<div class="columns" v-if="display_boxes || search_display_boxes">
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

  <!-- Box 13 -->
<div class="column">
    <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[12].label}}
          {{searchProbabilityValue(d_tactic[12].id)}}
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
               {{d_tactic[12].description}}
            </p>
          </b-collapse>
       
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[12].id), 'has-background-danger': !tactics.includes(d_tactic[12].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[12].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
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
          <b-message v-for="technique in d_tactic[12].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
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

    <!-- Box 14 -->
<div class="column">
    <div class="card">
      <header class="card-header is-flex is-flex-direction-column">
        <p class="card-header-title">
          {{d_tactic[13].label}}
          {{searchProbabilityValue(d_tactic[13].id)}}
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
               {{d_tactic[13].description}}
            </p>
          </b-collapse>
       
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      
      </header>
      <div v-if="display_boxes" class="card-content" :class="{ 'has-background-success': tactics.includes(d_tactic[13].id), 'has-background-danger': !tactics.includes(d_tactic[13].id) }">
        <div class="content">
          <b-message v-for="technique in d_tactic[13].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
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
          <b-message v-for="technique in d_tactic[13].d_techniques" :key="technique.label" :class="{ 'has-background-success': techniques.includes(technique.id), 'has-background-warning': !techniques.includes(technique.id) }">
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
              description: 'Adversaries may execute active reconnaissance scans to gather information that can be used during targeting. Active scans are those where the adversary probes victim infrastructure via network traffic, as opposed to other forms of reconnaissance that do not involve direct interaction.',
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
              label: 'Drive-by Compromise',
              description: 'Adversaries may gain access to a system through a user visiting a website over the normal course of browsing. With this technique, the user web browser is typically targeted for exploitation, but adversaries may also use compromised websites for non-exploitation behavior such as acquiring Application Access Token.'
            },
            {
              id: 'T1190',
              label: 'Exploit Public-Facing Application',
              description: 'Adversaries may attempt to take advantage of a weakness in an Internet-facing computer or program using software, data, or commands in order to cause unintended or unanticipated behavior. The weakness in the system can be a bug, a glitch, or a design vulnerability. These applications are often websites, but can include databases (like SQL), standard services (like SMB or SSH), network device administration and management protocols (like SNMP and Smart Install), and any other applications with Internet accessible open sockets, such as web servers and related services. Depending on the flaw being exploited this may include Exploitation for Defense Evasion.'
            },
            {
              id: 'T1133',
              label: 'External Remote Services',
              description: 'Adversaries may leverage external-facing remote services to initially access and/or persist within a network. Remote services such as VPNs, Citrix, and other access mechanisms allow users to connect to internal enterprise network resources from external locations. There are often remote service gateways that manage connections and credential authentication for these services. Services such as Windows Remote Management and VNC can also be used externally.'
            },
            {
              id: 'T1200', 
              label: 'Hardware Additions',
              description: 'Adversaries may introduce computer accessories, networking hardware, or other computing devices into a system or network that can be used as a vector to gain access. Rather than just connecting and distributing payloads via removable storage (i.e. Replication Through Removable Media), more robust hardware additions can be used to introduce new functionalities and/or features into a system that can then be abused.'
            },
            {
              id: 'T1566',
              label: 'Phishing',
              description: 'Adversaries may send phishing messages to gain access to victim systems. All forms of phishing are electronically delivered social engineering. Phishing can be targeted, known as spearphishing. In spearphishing, a specific individual, company, or industry will be targeted by the adversary. More generally, adversaries can conduct non-targeted phishing, such as in mass malware spam campaigns.'
            },
            {
              id: 'T1091',
              label: 'Replication Through Removable Media',
              description: 'Adversaries may move onto systems, possibly those on disconnected or air-gapped networks, by copying malware to removable media and taking advantage of Autorun features when the media is inserted into a system and executes. In the case of Lateral Movement, this may occur through modification of executable files stored on removable media or by copying malware and renaming it to look like a legitimate file to trick users into executing it on a separate system. In the case of Initial Access, this may occur through manual manipulation of the media, modification of systems used to initially format the media, or modification to the media firmware itself.'
            },
            {
              id: 'T1195', 
              label: 'Supply Chain Compromise',
              description: 'Adversaries may manipulate products or product delivery mechanisms prior to receipt by a final consumer for the purpose of data or system compromise.'
            },
            {
              id: 'T1199',
              label: 'Trusted Relationship',
              description: 'Adversaries may breach or otherwise leverage organizations who have access to intended victims. Access through trusted third party relationship exploits an existing connection that may not be protected or receives less scrutiny than standard mechanisms of gaining access to a network.'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts',
              description: 'Adversaries may obtain and abuse credentials of existing accounts as a means of gaining Initial Access, Persistence, Privilege Escalation, or Defense Evasion. Compromised credentials may be used to bypass access controls placed on various resources on systems within the network and may even be used for persistent access to remote systems and externally available services, such as VPNs, Outlook Web Access and remote desktop. Compromised credentials may also grant an adversary increased privilege to specific systems or access to restricted areas of the network. Adversaries may choose not to use malware or tools in conjunction with the legitimate access those credentials provide to make it harder to detect their presence.'
            }
          ]
        },
        {
          id: 'TA0002',
          label: '4) Execution (TA0002)',
          description: 'The adversary is trying to run malicious code.',
          d_techniques: [
            {
              id: 'T1059', 
              label: 'Command and Scripting Interpreter',
              description: 'Adversaries may abuse command and script interpreters to execute commands, scripts, or binaries. These interfaces and languages provide ways of interacting with computer systems and are a common feature across many different platforms. Most systems come with some built-in command-line interface and scripting capabilities, for example, macOS and Linux distributions include some flavor of Unix Shell while Windows installations include the Windows Command Shell and PowerShell.'
            },
            {
              id: 'T1609',
              label: 'Container Administration Command',
              description: 'Adversaries may abuse a container administration service to execute commands within a container. A container administration service such as the Docker daemon, the Kubernetes API server, or the kubelet may allow remote management of containers within an environment.'
            },
            {
              id: 'T1610',
              label: 'Deploy Container', 
              description: 'Adversaries may deploy a container into an environment to facilitate execution or evade defenses. In some cases, adversaries may deploy a new container to execute processes associated with a particular image or deployment, such as processes that execute or download malware. In others, an adversary may deploy a new container configured without network rules, user limitations, etc. to bypass existing defenses within the environment.'
            },
            {
              id: 'T1203', 
              label: 'Exploitation for Client Execution',
              description: 'Adversaries may exploit software vulnerabilities in client applications to execute code. Vulnerabilities can exist in software due to unsecure coding practices that can lead to unanticipated behavior. Adversaries can take advantage of certain vulnerabilities through targeted exploitation for the purpose of arbitrary code execution. Oftentimes the most valuable exploits to an offensive toolkit are those that can be used to obtain code execution on a remote system because they can be used to gain access to that system. Users will expect to see files related to the applications they commonly used to do work, so they are a useful target for exploit research and development because of their high utility.'
            },
            {
              id: 'T1559',
              label: 'Inter-Process Communication',
              description: 'Adversaries may abuse inter-process communication (IPC) mechanisms for local code or command execution. IPC is typically used by processes to share data, communicate with each other, or synchronize execution. IPC is also commonly used to avoid situations such as deadlocks, which occurs when processes are stuck in a cyclic waiting pattern.'
            },
            {
              id: 'T1106',
              label: 'Native API',
              description: 'Adversaries may interact with the native OS application programming interface (API) to execute behaviors. Native APIs provide a controlled means of calling low-level OS services within the kernel, such as those involving hardware/devices, memory, and processes. These native APIs are leveraged by the OS during system boot (when other system components are not yet initialized) as well as carrying out tasks and requests during routine operations.'
            },
            {
              id: 'T1053', 
              label: 'Scheduled Task/Job',
              description: 'Adversaries may abuse task scheduling functionality to facilitate initial or recurring execution of malicious code. Utilities exist within all major operating systems to schedule programs or scripts to be executed at a specified date and time. A task can also be scheduled on a remote system, provided the proper authentication is met (ex: RPC and file and printer sharing in Windows environments). Scheduling a task on a remote system typically may require being a member of an admin or otherwise privileged group on the remote system.'
            },
            {
              id: 'T1129',
              label: 'Shared Modules',
              description: 'Adversaries may execute malicious payloads via loading shared modules. The Windows module loader can be instructed to load DLLs from arbitrary local paths and arbitrary Universal Naming Convention (UNC) network paths. This functionality resides in NTDLL.dll and is part of the Windows Native API which is called from functions like CreateProcess, LoadLibrary, etc. of the Win32 API.'
            },
            {
              id: 'T1072', 
              label: 'Software Deployment Tools',
              description: 'Adversaries may gain access to and use third-party software suites installed within an enterprise network, such as administration, monitoring, and deployment systems, to move laterally through the network. Third-party applications and software deployment systems may be in use in the network environment for administration purposes (e.g., SCCM, HBSS, Altiris, etc.).'
            },
            {
              id: 'T1569', 
              label: 'System Services',
              description: 'Adversaries may abuse system services or daemons to execute commands or programs. Adversaries can execute malicious content by interacting with or creating services either locally or remotely. Many services are set to run at boot, which can aid in achieving persistence (Create or Modify System Process), but adversaries can also abuse services for one-time or temporary execution.'
            },
            {
              id: 'T1204',
              label: 'User Execution',
              description: 'An adversary may rely upon specific actions by a user in order to gain execution. Users may be subjected to social engineering to get them to execute malicious code by, for example, opening a malicious document file or link. These user actions will typically be observed as follow-on behavior from forms of Phishing.'
            },
            {
              id: 'T1047', 
              label: 'Windows Management Instrumentation',
              description: 'Adversaries may abuse Windows Management Instrumentation (WMI) to execute malicious commands and payloads. WMI is an administration feature that provides a uniform environment to access Windows system components. The WMI service enables both local and remote access, though the latter is facilitated by Remote Services such as Distributed Component Object Model (DCOM) and Windows Remote Management (WinRM). Remote WMI over DCOM operates using port 135, whereas WMI over WinRM operates over port 5985 when using HTTP and 5986 for HTTPS.'
            }
          ]
        },
        {
          id: 'TA0003', 
          label: '3) Persistence (TA0003)',
          description: 'The adversary is trying to maintain their foothold.',
          d_techniques: [
            {
              id: 'T1098', 
              label: 'Account Manipulation',
              description: 'Adversaries may manipulate accounts to maintain access to victim systems. Account manipulation may consist of any action that preserves adversary access to a compromised account, such as modifying credentials or permission groups. These actions could also include account activity designed to subvert security policies, such as performing iterative password updates to bypass password duration policies and preserve the life of compromised credentials.'
            },
            {
              id: 'T1197',
              label: 'BITS Jobs',
              description: 'Adversaries may abuse BITS jobs to persistently execute or clean up after malicious payloads. Windows Background Intelligent Transfer Service (BITS) is a low-bandwidth, asynchronous file transfer mechanism exposed through Component Object Model (COM). BITS is commonly used by updaters, messengers, and other applications preferred to operate in the background (using available idle bandwidth) without interrupting other networked applications. File transfer tasks are implemented as BITS jobs, which contain a queue of one or more file operations.'
            },
            {
              id: 'T1547',
              label: 'Boot or Logon Autostart Execution',
              description: 'Adversaries may configure system settings to automatically execute a program during system boot or logon to maintain persistence or gain higher-level privileges on compromised systems. Operating systems may have mechanisms for automatically running a program on system boot or account logon. These mechanisms may include automatically executing programs that are placed in specially designated directories or are referenced by repositories that store configuration information, such as the Windows Registry. An adversary may achieve the same goal by modifying or extending features of the kernel.'
            },
            {
              id: 'T1037', 
              label: 'Boot or Logon Initialization Scripts',
              description: 'Adversaries may use scripts automatically executed at boot or logon initialization to establish persistence. Initialization scripts can be used to perform administrative functions, which may often execute other programs or send information to an internal logging server. These scripts can vary based on operating system and whether applied locally or remotely.'
            },
            {
              id: 'T1176',
              label: 'Browser Extensions',
              description: 'Adversaries may abuse Internet browser extensions to establish persistent access to victim systems. Browser extensions or plugins are small programs that can add functionality and customize aspects of Internet browsers. They can be installed directly or through a browser app store and generally have access and permissions to everything that the browser can access.'
            },
            {
              id: 'T1554',
              label: 'Compromise Client Software Binary',
              description: 'Adversaries may modify client software binaries to establish persistent access to systems. Client software enables users to access services provided by a server. Common client software types are SSH clients, FTP clients, email clients, and web browsers.'
            },
            {
              id: 'T1136', 
              label: 'Create Account',
              description: 'Adversaries may create an account to maintain access to victim systems. With a sufficient level of access, creating such accounts may be used to establish secondary credentialed access that do not require persistent remote access tools to be deployed on the system.'
            },
            {
              id: 'T1543',
              label: 'Create or Modify System Process',
              description: 'Adversaries may create or modify system-level processes to repeatedly execute malicious payloads as part of persistence. When operating systems boot up, they can start processes that perform background system functions. On Windows and Linux, these system processes are referred to as services. On macOS, launchd processes known as Launch Daemon and Launch Agent are run to finish system initialization and load user specific parameters.'
            },
            {
              id: 'T1546', 
              label: 'Event Triggered Execution',
              description: 'Adversaries may establish persistence and/or elevate privileges using system mechanisms that trigger execution based on specific events. Various operating systems have means to monitor and subscribe to events such as logons or other user activity such as running specific applications/binaries.'
            },
            {
              id: 'T1133', 
              label: 'External Remote Services',
              description: 'Adversaries may leverage external-facing remote services to initially access and/or persist within a network. Remote services such as VPNs, Citrix, and other access mechanisms allow users to connect to internal enterprise network resources from external locations. There are often remote service gateways that manage connections and credential authentication for these services. Services such as Windows Remote Management and VNC can also be used externally.'
            },
            {
              id: 'T1574',
              label: 'Hijack Execution Flow',
              description: 'Adversaries may execute their own malicious payloads by hijacking the way operating systems run programs. Hijacking execution flow can be for the purposes of persistence, since this hijacked execution may reoccur over time. Adversaries may also use these mechanisms to elevate privileges or evade defenses, such as application control or other restrictions on execution.'
            },
            {
              id: 'T1525', 
              label: 'Implant Internal Image',
              description: 'Adversaries may implant cloud or container images with malicious code to establish persistence after gaining access to an environment. Amazon Web Services (AWS) Amazon Machine Images (AMIs), Google Cloud Platform (GCP) Images, and Azure Images as well as popular container runtimes such as Docker can be implanted or backdoored. Unlike Upload Malware, this technique focuses on adversaries implanting an image in a registry within a victim’s environment. Depending on how the infrastructure is provisioned, this could provide persistent access if the infrastructure provisioning tool is instructed to always use the latest image.'
            },
            {
              id: 'T1556',
              label: 'Modify Authentication Process',
              description: 'Adversaries may modify authentication mechanisms and processes to access user credentials or enable otherwise unwarranted access to accounts. The authentication process is handled by mechanisms, such as the Local Security Authentication Server (LSASS) process and the Security Accounts Manager (SAM) on Windows, pluggable authentication modules (PAM) on Unix-based systems, and authorization plugins on MacOS systems, responsible for gathering, storing, and validating credentials. By modifying an authentication process, an adversary may be able to authenticate to a service or system without using Valid Accounts.'
            },
            {
              id: 'T1137', 
              label: 'Office Application Startup',
              description: 'Adversaries may leverage Microsoft Office-based applications for persistence between startups. Microsoft Office is a fairly common application suite on Windows-based operating systems within an enterprise network. There are multiple mechanisms that can be used with Office for persistence when an Office-based application is started; this can include the use of Office Template Macros and add-ins.'
            },
            {
              id: 'T1542',
              label: 'Pre-OS Boot',
              description: 'Adversaries may abuse Pre-OS Boot mechanisms as a way to establish persistence on a system. During the booting process of a computer, firmware and various startup services are loaded before the operating system. These programs control flow of execution before the operating system takes control.'
            },
            {
              id: 'T1053', 
              label: 'Scheduled Task/Job',
              description: 'Adversaries may abuse task scheduling functionality to facilitate initial or recurring execution of malicious code. Utilities exist within all major operating systems to schedule programs or scripts to be executed at a specified date and time. A task can also be scheduled on a remote system, provided the proper authentication is met (ex: RPC and file and printer sharing in Windows environments). Scheduling a task on a remote system typically may require being a member of an admin or otherwise privileged group on the remote system.'
            },
            {
              id: 'T1505', 
              label: 'Server Software Component',
              description: 'Adversaries may abuse legitimate extensible development features of servers to establish persistent access to systems. Enterprise server applications may include features that allow developers to write and install software or scripts to extend the functionality of the main application. Adversaries may install malicious components to extend and abuse server applications.'
            },
            {
              id: 'T1205',
              label: 'Traffic Signaling',
              description: 'Adversaries may use traffic signaling to hide open ports or other malicious functionality used for persistence or command and control. Traffic signaling involves the use of a magic value or sequence that must be sent to a system to trigger a special response, such as opening a closed port or executing a malicious task. This may take the form of sending a series of packets with certain characteristics before a port will be opened that the adversary can use for command and control. Usually this series of packets consists of attempted connections to a predefined sequence of closed ports (i.e. Port Knocking), but can involve unusual flags, specific strings, or other unique characteristics. After the sequence is completed, opening a port may be accomplished by the host-based firewall, but could also be implemented by custom software.'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts',
              description: 'Adversaries may obtain and abuse credentials of existing accounts as a means of gaining Initial Access, Persistence, Privilege Escalation, or Defense Evasion. Compromised credentials may be used to bypass access controls placed on various resources on systems within the network and may even be used for persistent access to remote systems and externally available services, such as VPNs, Outlook Web Access and remote desktop. Compromised credentials may also grant an adversary increased privilege to specific systems or access to restricted areas of the network. Adversaries may choose not to use malware or tools in conjunction with the legitimate access those credentials provide to make it harder to detect their presence.'
            }
          ]
        },
        {
          id: 'TA0004', 
          label: '4) Privilege Escalation (TA0004)',
          description: 'The adversary is trying to gain higher-level permissions.',
          d_techniques: [
            {
              id: 'T1548', 
              label: 'Abuse Elevation Control Mechanism',
              description: 'Adversaries may circumvent mechanisms designed to control elevate privileges to gain higher-level permissions. Most modern systems contain native elevation control mechanisms that are intended to limit privileges that a user can perform on a machine. Authorization has to be granted to specific users in order to perform tasks that can be considered of higher risk. An adversary can perform several methods to take advantage of built-in control mechanisms in order to escalate privileges on a system.'
            },
            {
              id: 'T1134',
              label: 'Access Token Manipulation',
              description: 'Adversaries may modify access tokens to operate under a different user or system security context to perform actions and bypass access controls. Windows uses access tokens to determine the ownership of a running process. A user can manipulate access tokens to make a running process appear as though it is the child of a different process or belongs to someone other than the user that started the process. When this occurs, the process also takes on the security context associated with the new token.'
            },
            {
              id: 'T1547',
              label: 'Boot or Logon Autostart Execution',
              description: 'Adversaries may configure system settings to automatically execute a program during system boot or logon to maintain persistence or gain higher-level privileges on compromised systems. Operating systems may have mechanisms for automatically running a program on system boot or account logon. These mechanisms may include automatically executing programs that are placed in specially designated directories or are referenced by repositories that store configuration information, such as the Windows Registry. An adversary may achieve the same goal by modifying or extending features of the kernel.'
            },
            {
              id: 'T1037', 
              label: 'Boot or Logon Initialization Scripts',
              description: 'Adversaries may use scripts automatically executed at boot or logon initialization to establish persistence. Initialization scripts can be used to perform administrative functions, which may often execute other programs or send information to an internal logging server. These scripts can vary based on operating system and whether applied locally or remotely.'
            },
            {
              id: 'T1543',
              label: 'Create or Modify System Process',
              description: 'Adversaries may create or modify system-level processes to repeatedly execute malicious payloads as part of persistence. When operating systems boot up, they can start processes that perform background system functions. On Windows and Linux, these system processes are referred to as services. On macOS, launchd processes known as Launch Daemon and Launch Agent are run to finish system initialization and load user specific parameters.'
            },
            {
              id: 'T1484',
              label: 'Domain Policy Modification',
              description: 'Adversaries may modify the configuration settings of a domain to evade defenses and/or escalate privileges in domain environments. Domains provide a centralized means of managing how computer resources (ex: computers, user accounts) can act, and interact with each other, on a network. The policy of the domain also includes configuration settings that may apply between domains in a multi-domain/forest environment. Modifications to domain settings may include altering domain Group Policy Objects (GPOs) or changing trust settings for domains, including federation trusts.'
            },
            {
              id: 'T1611', 
              label: 'Escape to Host',
              description: 'Adversaries may break out of a container to gain access to the underlying host. This can allow an adversary access to other containerized resources from the host level or to the host itself. In principle, containerized resources should provide a clear separation of application functionality and be isolated from the host environment.'
            },
            {
              id: 'T1546',
              label: 'Event Triggered Execution',
              description: 'Adversaries may establish persistence and/or elevate privileges using system mechanisms that trigger execution based on specific events. Various operating systems have means to monitor and subscribe to events such as logons or other user activity such as running specific applications/binaries.'
            },
            {
              id: 'T1068', 
              label: 'Exploitation for Privilege Escalation',
              description: 'Adversaries may exploit software vulnerabilities in an attempt to elevate privileges. Exploitation of a software vulnerability occurs when an adversary takes advantage of a programming error in a program, service, or within the operating system software or kernel itself to execute adversary-controlled code. Security constructs such as permission levels will often hinder access to information and use of certain techniques, so adversaries will likely need to perform privilege escalation to include use of software exploitation to circumvent those restrictions.'
            },
            {
              id: 'T1055', 
              label: 'Process Injection',
              description: 'Adversaries may inject code into processes in order to evade process-based defenses as well as possibly elevate privileges. Process injection is a method of executing arbitrary code in the address space of a separate live process. Running code in the context of another process may allow access to the process memory, system/network resources, and possibly elevated privileges. Execution via process injection may also evade detection from security products since the execution is masked under a legitimate process.'
            },
            {
              id: 'T1053',
              label: 'Scheduled Task/Job',
              description: 'Adversaries may abuse task scheduling functionality to facilitate initial or recurring execution of malicious code. Utilities exist within all major operating systems to schedule programs or scripts to be executed at a specified date and time. A task can also be scheduled on a remote system, provided the proper authentication is met (ex: RPC and file and printer sharing in Windows environments). Scheduling a task on a remote system typically may require being a member of an admin or otherwise privileged group on the remote system.'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts',
              description: 'Adversaries may obtain and abuse credentials of existing accounts as a means of gaining Initial Access, Persistence, Privilege Escalation, or Defense Evasion. Compromised credentials may be used to bypass access controls placed on various resources on systems within the network and may even be used for persistent access to remote systems and externally available services, such as VPNs, Outlook Web Access and remote desktop. Compromised credentials may also grant an adversary increased privilege to specific systems or access to restricted areas of the network. Adversaries may choose not to use malware or tools in conjunction with the legitimate access those credentials provide to make it harder to detect their presence.'
            }
          ]
        },
        {
          id: 'TA0005', 
          label: '5) Defense Evasion (TA0005)',
          description: 'The adversary is trying to avoid being detected.',
          d_techniques: [
            {
              id: 'T1548', 
              label: 'Abuse Elevation Control Mechanism',
              description: 'Adversaries may circumvent mechanisms designed to control elevate privileges to gain higher-level permissions. Most modern systems contain native elevation control mechanisms that are intended to limit privileges that a user can perform on a machine. Authorization has to be granted to specific users in order to perform tasks that can be considered of higher risk. An adversary can perform several methods to take advantage of built-in control mechanisms in order to escalate privileges on a system.'
            },
            {
              id: 'T1134',
              label: 'Access Token Manipulation',
              description: 'Adversaries may modify access tokens to operate under a different user or system security context to perform actions and bypass access controls. Windows uses access tokens to determine the ownership of a running process. A user can manipulate access tokens to make a running process appear as though it is the child of a different process or belongs to someone other than the user that started the process. When this occurs, the process also takes on the security context associated with the new token.'
            },
            {
              id: 'T1197',
              label: 'BITS Jobs',
              description: 'Adversaries may abuse BITS jobs to persistently execute or clean up after malicious payloads. Windows Background Intelligent Transfer Service (BITS) is a low-bandwidth, asynchronous file transfer mechanism exposed through Component Object Model (COM). BITS is commonly used by updaters, messengers, and other applications preferred to operate in the background (using available idle bandwidth) without interrupting other networked applications. File transfer tasks are implemented as BITS jobs, which contain a queue of one or more file operations.'
            },
            {
              id: 'T1612', 
              label: 'Build Image on Host',
              description: 'Adversaries may build a container image directly on a host to bypass defenses that monitor for the retrieval of malicious images from a public registry. A remote build request may be sent to the Docker API that includes a Dockerfile that pulls a vanilla base image, such as alpine, from a public or local registry and then builds a custom image upon it.'
            },
            {
              id: 'T1622',
              label: 'Debugger Evasion',
              description: '	Adversaries may employ various means to detect and avoid debuggers. Debuggers are typically used by defenders to trace and/or analyze the execution of potential malware payloads.'
            },
            {
              id: 'T1140',
              label: 'Deobfuscate/Decode Files or Information',
              description: 'Adversaries may use Obfuscated Files or Information to hide artifacts of an intrusion from analysis. They may require separate mechanisms to decode or deobfuscate that information depending on how they intend to use it. Methods for doing that include built-in functionality of malware or by using utilities present on the system.'
            },
            {
              id: 'T1610', 
              label: 'Deploy Container',
              description: 'Adversaries may deploy a container into an environment to facilitate execution or evade defenses. In some cases, adversaries may deploy a new container to execute processes associated with a particular image or deployment, such as processes that execute or download malware. In others, an adversary may deploy a new container configured without network rules, user limitations, etc. to bypass existing defenses within the environment.'
            },
            {
              id: 'T1006',
              label: 'Direct Volume Access',
              description: 'Adversaries may directly access a volume to bypass file access controls and file system monitoring. Windows allows programs to have direct access to logical volumes. Programs with direct access may read and write files directly from the drive by analyzing file system data structures. This technique bypasses Windows file access controls as well as file system monitoring tools.'
            },
            {
              id: 'T1484', 
              label: 'Domain Policy Modification',
              description: 'Adversaries may modify the configuration settings of a domain to evade defenses and/or escalate privileges in domain environments. Domains provide a centralized means of managing how computer resources (ex: computers, user accounts) can act, and interact with each other, on a network. The policy of the domain also includes configuration settings that may apply between domains in a multi-domain/forest environment. Modifications to domain settings may include altering domain Group Policy Objects (GPOs) or changing trust settings for domains, including federation trusts.'
            },
            {
              id: 'T1480', 
              label: 'Execution Guardrails',
              description: 'Adversaries may use execution guardrails to constrain execution or actions based on adversary supplied and environment specific conditions that are expected to be present on the target. Guardrails ensure that a payload only executes against an intended target and reduces collateral damage from an adversary’s campaign. Values an adversary can provide about a target system or environment to use as guardrails may include specific network share names, attached physical devices, files, joined Active Directory (AD) domains, and local/external IP addresses.'
            },
            {
              id: 'T1211',
              label: 'Exploitation for Defense Evasion',
              description: 'Adversaries may exploit a system or application vulnerability to bypass security features. Exploitation of a software vulnerability occurs when an adversary takes advantage of a programming error in a program, service, or within the operating system software or kernel itself to execute adversary-controlled code. Vulnerabilities may exist in defensive security software that can be used to disable or circumvent them.'
            },
            {
              id: 'T1222', 
              label: 'File and Directory Permissions Modification',
              description: 'Adversaries may modify file or directory permissions/attributes to evade access control lists (ACLs) and access protected files. File and directory permissions are commonly managed by ACLs configured by the file or directory owner, or users with the appropriate permissions. File and directory ACL implementations vary by platform, but generally explicitly designate which users or groups can perform which actions (read, write, execute, etc.).'
            },
            {
              id: 'T1564', 
              label: 'Hide Artifacts',
              description: 'Adversaries may attempt to hide artifacts associated with their behaviors to evade detection. Operating systems may have features to hide various artifacts, such as important system files and administrative task execution, to avoid disrupting user work environments and prevent users from changing files or features on the system. Adversaries may abuse these features to hide artifacts such as files, directories, user accounts, or other system activity to evade detection.'
            },
            {
              id: 'T1574', 
              label: 'Hijack Execution Flow',
              description: 'Adversaries may execute their own malicious payloads by hijacking the way operating systems run programs. Hijacking execution flow can be for the purposes of persistence, since this hijacked execution may reoccur over time. Adversaries may also use these mechanisms to elevate privileges or evade defenses, such as application control or other restrictions on execution.'
            },
            {
              id: 'T1562',
              label: 'Impair Defenses',
              description: 'Adversaries may maliciously modify components of a victim environment in order to hinder or disable defensive mechanisms. This not only involves impairing preventative defenses, such as firewalls and anti-virus, but also detection capabilities that defenders can use to audit activity and identify malicious behavior. This may also span both native defenses as well as supplemental capabilities installed by users and administrators'
            },
            {
              id: 'T1070', 
              label: 'Indicator Removal on Host',
              description: 'Adversaries may delete or modify artifacts generated on a host system to remove evidence of their presence or hinder defenses. Various artifacts may be created by an adversary or something that can be attributed to an adversary’s actions. Typically these artifacts are used as defensive indicators related to monitored events, such as strings from downloaded files, logs that are generated from user actions, and other data analyzed by defenders. Location, format, and type of artifact (such as command or login history) are often specific to each platform.'
            },
            {
              id: 'T1202',
              label: 'Indirect Command Execution',
              description: 'Adversaries may abuse utilities that allow for command execution to bypass security restrictions that limit the use of command-line interpreters. Various Windows utilities may be used to execute commands, possibly without invoking cmd. For example, Forfiles, the Program Compatibility Assistant (pcalua.exe), components of the Windows Subsystem for Linux (WSL), as well as other utilities may invoke the execution of programs and commands from a Command and Scripting Interpreter, Run window, or via scripts.'
            },
            {
              id: 'T1036', 
              label: 'Masquerading',
              description: 'Adversaries may attempt to manipulate features of their artifacts to make them appear legitimate or benign to users and/or security tools. Masquerading occurs when the name or location of an object, legitimate or malicious, is manipulated or abused for the sake of evading defenses and observation. This may include manipulating file metadata, tricking users into misidentifying the file type, and giving legitimate task or service names.'
            },
            {
              id: 'T1556',
              label: 'Modify Authentication Process',
              description: 'Adversaries may modify authentication mechanisms and processes to access user credentials or enable otherwise unwarranted access to accounts. The authentication process is handled by mechanisms, such as the Local Security Authentication Server (LSASS) process and the Security Accounts Manager (SAM) on Windows, pluggable authentication modules (PAM) on Unix-based systems, and authorization plugins on MacOS systems, responsible for gathering, storing, and validating credentials. By modifying an authentication process, an adversary may be able to authenticate to a service or system without using Valid Accounts.'
            },
            {
              id: 'T1578', 
              label: 'Modify Cloud Compute Infrastructure',
              description: 'An adversary may attempt to modify a cloud account compute service infrastructure to evade defenses. A modification to the compute service infrastructure can include the creation, deletion, or modification of one or more components such as compute instances, virtual machines, and snapshots.'
            },
            {
              id: 'T1112', 
              label: 'Modify Registry',
              description: 'Adversaries may interact with the Windows Registry to hide configuration information within Registry keys, remove information as part of cleaning up, or as part of other techniques to aid in persistence and execution.'
            },
            {
              id: 'T1601',
              label: 'Modify System Image',
              description: 'Adversaries may make changes to the operating system of embedded network devices to weaken defenses and provide new capabilities for themselves. On such devices, the operating systems are typically monolithic and most of the device functionality and capabilities are contained within a single file.'
            },
            {
              id: 'T1599', 
              label: 'Network Boundary Bridging',
              description: 'Adversaries may bridge network boundaries by compromising perimeter network devices or internal devices responsible for network segmentation. Breaching these devices may enable an adversary to bypass restrictions on traffic routing that otherwise separate trusted and untrusted networks.'
            },
            {
              id: 'T1027', 
              label: 'Obfuscated Files or Information',
              description: 'Adversaries may attempt to make an executable or file difficult to discover or analyze by encrypting, encoding, or otherwise obfuscating its contents on the system or in transit. This is common behavior that can be used across different platforms and the network to evade defenses.'
            },
            {
              id: 'T1647', 
              label: 'Plist File Modification',
              description: 'Adversaries may modify property list files (plist files) to enable other malicious activity, while also potentially evading and bypassing system defenses. macOS applications use plist files, such as the info.plist file, to store properties and configuration settings that inform the operating system how to handle the application at runtime. Plist files are structured metadata in key-value pairs formatted in XML based on Apple Core Foundation DTD. Plist files can be saved in text or binary format.'
            },
            {
              id: 'T1542',
              label: 'Pre-OS Boot',
              description: 'Adversaries may abuse Pre-OS Boot mechanisms as a way to establish persistence on a system. During the booting process of a computer, firmware and various startup services are loaded before the operating system. These programs control flow of execution before the operating system takes control.'
            },
            {
              id: 'T1055', 
              label: 'Process Injection',
              description: 'Adversaries may inject code into processes in order to evade process-based defenses as well as possibly elevate privileges. Process injection is a method of executing arbitrary code in the address space of a separate live process. Running code in the context of another process may allow access to the process memory, system/network resources, and possibly elevated privileges. Execution via process injection may also evade detection from security products since the execution is masked under a legitimate process.'
            },
            {
              id: 'T1620', 
              label: 'Reflective Code Loading',
              description: 'Adversaries may reflectively load code into a process in order to conceal the execution of malicious payloads. Reflective loading involves allocating then executing payloads directly within the memory of the process, vice creating a thread or process backed by a file path on disk. Reflectively loaded payloads may be compiled binaries, anonymous files (only present in RAM), or just snubs of fileless executable code (ex: position-independent shellcode).'
            },
            {
              id: 'T1207',
              label: 'Rogue Domain Controller',
              description: 'Adversaries may register a rogue Domain Controller to enable manipulation of Active Directory data. DCShadow may be used to create a rogue Domain Controller (DC). DCShadow is a method of manipulating Active Directory (AD) data, including objects and schemas, by registering (or reusing an inactive registration) and simulating the behavior of a DC. Once registered, a rogue DC may be able to inject and replicate changes into AD infrastructure for any domain object, including credentials and keys.'
            },
            {
              id: 'T1014', 
              label: 'Rootkit',
              description: 'Adversaries may use rootkits to hide the presence of programs, files, network connections, services, drivers, and other system components. Rootkits are programs that hide the existence of malware by intercepting/hooking and modifying operating system API calls that supply system information.'
            },
            {
              id: 'T1553', 
              label: 'Subvert Trust Controls',
              description: 'Adversaries may undermine security controls that will either warn users of untrusted activity or prevent execution of untrusted programs. Operating systems and security products may contain mechanisms to identify programs or websites as possessing some level of trust. Examples of such features would include a program being allowed to run because it is signed by a valid code signing certificate, a program prompting the user with a warning because it has an attribute set from being downloaded from the Internet, or getting an indication that you are about to connect to an untrusted site.'
            },
            {
              id: 'T1218',
              label: 'System Binary Proxy Execution',
              description: 'Adversaries may bypass process and/or signature-based defenses by proxying execution of malicious content with signed, or otherwise trusted, binaries. Binaries used in this technique are often Microsoft-signed files, indicating that they have been either downloaded from Microsoft or are already native in the operating system. Binaries signed with trusted digital certificates can typically execute on Windows systems protected by digital signature validation. Several Microsoft signed binaries that are default on Windows installations can be used to proxy execution of other files or commands.'
            },
            {
              id: 'T1216', 
              label: 'System Script Proxy Execution',
              description: 'Adversaries may use trusted scripts, often signed with certificates, to proxy the execution of malicious files. Several Microsoft signed scripts that have been downloaded from Microsoft or are default on Windows installations can be used to proxy execution of other files. This behavior may be abused by adversaries to execute malicious files that could bypass application control and signature validation on systems.'
            },
            {
              id: 'T1221', 
              label: 'Template Injection',
              description: 'Adversaries may create or modify references in user document templates to conceal malicious code or force authentication attempts. For example, Microsoft’s Office Open XML (OOXML) specification defines an XML-based format for Office documents (.docx, xlsx, .pptx) to replace older binary formats (.doc, .xls, .ppt). OOXML files are packed together ZIP archives compromised of various XML files, referred to as parts, containing properties that collectively define how a document is rendered.'
            },
            {
              id: 'T1205', 
              label: 'Traffic Signaling',
              description: 'Adversaries may use traffic signaling to hide open ports or other malicious functionality used for persistence or command and control. Traffic signaling involves the use of a magic value or sequence that must be sent to a system to trigger a special response, such as opening a closed port or executing a malicious task. This may take the form of sending a series of packets with certain characteristics before a port will be opened that the adversary can use for command and control. Usually this series of packets consists of attempted connections to a predefined sequence of closed ports (i.e. Port Knocking), but can involve unusual flags, specific strings, or other unique characteristics. After the sequence is completed, opening a port may be accomplished by the host-based firewall, but could also be implemented by custom software.'
            },
            {
              id: 'T1127',
              label: 'Trusted Developer Utilities Proxy Execution',
              description: 'Adversaries may take advantage of trusted developer utilities to proxy execution of malicious payloads. There are many utilities used for software development related tasks that can be used to execute code in various forms to assist in development, debugging, and reverse engineering. These utilities may often be signed with legitimate certificates that allow them to execute on a system and proxy execution of malicious code through a trusted process that effectively bypasses application control solutions.'
            },
            {
              id: 'T1535', 
              label: 'Unused/Unsupported Cloud Regions',
              description: 'Adversaries may create cloud instances in unused geographic service regions in order to evade detection. Access is usually obtained through compromising accounts used to manage cloud infrastructure.'
            },
            {
              id: 'T1550', 
              label: 'Use Alternate Authentication Material',
              description: 'Adversaries may use alternate authentication material, such as password hashes, Kerberos tickets, and application access tokens, in order to move laterally within an environment and bypass normal system access controls.'
            },
            {
              id: 'T1078', 
              label: 'Valid Accounts',
              description: 'Adversaries may obtain and abuse credentials of existing accounts as a means of gaining Initial Access, Persistence, Privilege Escalation, or Defense Evasion. Compromised credentials may be used to bypass access controls placed on various resources on systems within the network and may even be used for persistent access to remote systems and externally available services, such as VPNs, Outlook Web Access and remote desktop. Compromised credentials may also grant an adversary increased privilege to specific systems or access to restricted areas of the network. Adversaries may choose not to use malware or tools in conjunction with the legitimate access those credentials provide to make it harder to detect their presence.'
            },
            {
              id: 'T1497',
              label: 'Virtualization/Sandbox Evasion',
              description: 'Adversaries may employ various means to detect and avoid virtualization and analysis environments. This may include changing behaviors based on the results of checks for the presence of artifacts indicative of a virtual machine environment (VME) or sandbox. If the adversary detects a VME, they may alter their malware to disengage from the victim or conceal the core functions of the implant. They may also search for VME artifacts before dropping secondary or additional payloads. Adversaries may use the information learned from Virtualization/Sandbox Evasion during automated discovery to shape follow-on behaviors.'
            },
            {
              id: 'T1600', 
              label: 'Weaken Encryption',
              description: 'Adversaries may compromise a network device’s encryption capability in order to bypass encryption that would otherwise protect data communications.'
            },
            {
              id: 'T1220', 
              label: 'XSL Script Processing',
              description: 'Adversaries may bypass application control and obscure execution of code by embedding scripts inside XSL files. Extensible Stylesheet Language (XSL) files are commonly used to describe the processing and rendering of data within XML files. To support complex operations, the XSL standard includes support for embedded scripting in various languages.'
            }
          ]
        },
        {
          id: 'TA0006',
          label: '6) Credential Access (TA0006)',
          description: 'The adversary is trying to steal account names and passwords.',
          d_techniques: [
            {
              id: 'T1557', 
              label: 'Adversary-in-the-Middle',
              description: 'Adversaries may attempt to position themselves between two or more networked devices using an adversary-in-the-middle (AiTM) technique to support follow-on behaviors such as Network Sniffing or Transmitted Data Manipulation. By abusing features of common networking protocols that can determine the flow of network traffic (e.g. ARP, DNS, LLMNR, etc.), adversaries may force a device to communicate through an adversary controlled system so they can collect information or perform additional actions.'
            },
            {
              id: 'T1110',
              label: 'Brute Force',
              description: 'Adversaries may use brute force techniques to gain access to accounts when passwords are unknown or when password hashes are obtained. Without knowledge of the password for an account or set of accounts, an adversary may systematically guess the password using a repetitive or iterative mechanism. Brute forcing passwords can take place via interaction with a service that will check the validity of those credentials or offline against previously acquired credential data, such as password hashes.'
            },
            {
              id: 'T1555',
              label: 'Credentials from Password Stores',
              description: 'Adversaries may search for common password storage locations to obtain user credentials. Passwords are stored in several places on a system, depending on the operating system or application holding the credentials. There are also specific applications that store passwords to make it easier for users manage and maintain. Once credentials are obtained, they can be used to perform lateral movement and access restricted information.'
            },
            {
              id: 'T1212', 
              label: 'Exploitation for Credential Access',
              description: 'Adversaries may exploit software vulnerabilities in an attempt to collect credentials. Exploitation of a software vulnerability occurs when an adversary takes advantage of a programming error in a program, service, or within the operating system software or kernel itself to execute adversary-controlled code. Credentialing and authentication mechanisms may be targeted for exploitation by adversaries as a means to gain access to useful credentials or circumvent the process to gain access to systems. One example of this is MS14-068, which targets Kerberos and can be used to forge Kerberos tickets using domain user permissions. Exploitation for credential access may also result in Privilege Escalation depending on the process targeted or credentials obtained.'
            },
            {
              id: 'T1187',
              label: 'Forced Authentication',
              description: 'Adversaries may gather credential material by invoking or forcing a user to automatically provide authentication information through a mechanism in which they can intercept.'
            },
            {
              id: 'T1606',
              label: 'Forge Web Credentials',
              description: 'Adversaries may forge credential materials that can be used to gain access to web applications or Internet services. Web applications and services (hosted in cloud SaaS environments or on-premise servers) often use session cookies, tokens, or other materials to authenticate and authorize user access.'
            },
            {
              id: 'T1056', 
              label: 'Input Capture',
              description: 'Adversaries may forge credential materials that can be used to gain access to web applications or Internet services. Web applications and services (hosted in cloud SaaS environments or on-premise servers) often use session cookies, tokens, or other materials to authenticate and authorize user access.'
            },
            {
              id: 'T1556',
              label: 'Modify Authentication Process',
              description: 'Adversaries may modify authentication mechanisms and processes to access user credentials or enable otherwise unwarranted access to accounts. The authentication process is handled by mechanisms, such as the Local Security Authentication Server (LSASS) process and the Security Accounts Manager (SAM) on Windows, pluggable authentication modules (PAM) on Unix-based systems, and authorization plugins on MacOS systems, responsible for gathering, storing, and validating credentials. By modifying an authentication process, an adversary may be able to authenticate to a service or system without using Valid Accounts.'
            },
            {
              id: 'T1111', 
              label: 'Multi-Factor Authentication Interception',
              description: 'Adversaries may target multi-factor authentication (MFA) mechanisms, (I.e., smart cards, token generators, etc.) to gain access to credentials that can be used to access systems, services, and network resources. Use of MFA is recommended and provides a higher level of security than user names and passwords alone, but organizations should be aware of techniques that could be used to intercept and bypass these security mechanisms.'
            },
            {
              id: 'T1621', 
              label: 'Multi-Factor Authentication Request Generation',
              description: 'Adversaries may attempt to bypass multi-factor authentication (MFA) mechanisms and gain access to accounts by generating MFA requests sent to users.'
            },
            {
              id: 'T1040',
              label: 'Network Sniffing',
              description: 'Adversaries may sniff network traffic to capture information about an environment, including authentication material passed over the network. Network sniffing refers to using the network interface on a system to monitor or capture information sent over a wired or wireless connection. An adversary may place a network interface into promiscuous mode to passively access data in transit over the network, or use span ports to capture a larger amount of data.'
            },
            {
              id: 'T1003', 
              label: 'OS Credential Dumping',
              description: 'Adversaries may attempt to dump credentials to obtain account login and credential material, normally in the form of a hash or a clear text password, from the operating system and software. Credentials can then be used to perform Lateral Movement and access restricted information.'
            },
            {
              id: 'T1528', 
              label: 'Steal Application Access Token',
              description: 'Adversaries can steal application access tokens as a means of acquiring credentials to access remote systems and resources.'
            },
            {
              id: 'T1558', 
              label: 'Steal or Forge Kerberos Tickets',
              description: 'Adversaries may attempt to subvert Kerberos authentication by stealing or forging Kerberos tickets to enable Pass the Ticket. Kerberos is an authentication protocol widely used in modern Windows domain environments. In Kerberos environments, referred to as "realms", there are three basic participants: client, service, and Key Distribution Center (KDC). Clients request access to a service and through the exchange of Kerberos tickets, originating from KDC, they are granted access after having successfully authenticated. The KDC is responsible for both authentication and ticket granting. Adversaries may attempt to abuse Kerberos by stealing tickets or forging tickets to enable unauthorized access.'
            },
            {
              id: 'T1539',
              label: 'Steal Web Session Cookie',
              description: 'An adversary may steal web application or service session cookies and use them to gain access to web applications or Internet services as an authenticated user without needing credentials. Web applications and services often use session cookies as an authentication token after a user has authenticated to a website.'
            },
            {
              id: 'T1552', 
              label: 'Unsecured Credentials',
              description: 'Adversaries may search compromised systems to find and obtain insecurely stored credentials. These credentials can be stored and/or misplaced in many locations on a system, including plaintext files (e.g. Bash History), operating system or application-specific repositories (e.g. Credentials in Registry), or other specialized files/artifacts (e.g. Private Keys).'
            }
          ]
        },
        {
          id: 'TA0007',
          label: '9) Discovery (TA0007)',
          description: 'The adversary is trying to figure out your environment.',
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
              label: 'Debugger Evasion',
              description: '	Adversaries may employ various means to detect and avoid debuggers. Debuggers are typically used by defenders to trace and/or analyze the execution of potential malware payloads.'
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
              label: 'Virtualization/Sandbox Evasion',
              description: 'Adversaries may employ various means to detect and avoid virtualization and analysis environments. This may include changing behaviors based on the results of checks for the presence of artifacts indicative of a virtual machine environment (VME) or sandbox. If the adversary detects a VME, they may alter their malware to disengage from the victim or conceal the core functions of the implant. They may also search for VME artifacts before dropping secondary or additional payloads. Adversaries may use the information learned from Virtualization/Sandbox Evasion during automated discovery to shape follow-on behaviors.'
            }
          ]
        },
        {
          id: 'TA0008', 
          label: '10) Lateral Movement (TA0008)',
          description: 'The adversary is trying to move through your environment.',
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
          description: 'The adversary is trying to gather data of interest to their goal.',
          d_techniques: [
            {
              id: 'T1557', 
              label: 'Adversary-in-the-Middle',
              description: 'Adversaries may attempt to position themselves between two or more networked devices using an adversary-in-the-middle (AiTM) technique to support follow-on behaviors such as Network Sniffing or Transmitted Data Manipulation. By abusing features of common networking protocols that can determine the flow of network traffic (e.g. ARP, DNS, LLMNR, etc.), adversaries may force a device to communicate through an adversary controlled system so they can collect information or perform additional actions.'
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
              label: 'Input Capture',
              description: 'Adversaries may forge credential materials that can be used to gain access to web applications or Internet services. Web applications and services (hosted in cloud SaaS environments or on-premise servers) often use session cookies, tokens, or other materials to authenticate and authorize user access.'
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
          description: 'The adversary is trying to communicate with compromised systems to control them.',
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
              label: 'Traffic Signaling',
              description: 'Adversaries may use traffic signaling to hide open ports or other malicious functionality used for persistence or command and control. Traffic signaling involves the use of a magic value or sequence that must be sent to a system to trigger a special response, such as opening a closed port or executing a malicious task. This may take the form of sending a series of packets with certain characteristics before a port will be opened that the adversary can use for command and control. Usually this series of packets consists of attempted connections to a predefined sequence of closed ports (i.e. Port Knocking), but can involve unusual flags, specific strings, or other unique characteristics. After the sequence is completed, opening a port may be accomplished by the host-based firewall, but could also be implemented by custom software.'
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
          description: 'The adversary is trying to steal data',
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
          label: '14) Impact (TA0040)',
          description: 'The adversary is trying to manipulate, interrupt, or destroy your systems and data.',
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
      api_url: "https://api-cti.herokuapp.com/classification", // change api address 
      search_url: "https://api-cti.herokuapp.com/searchattack",
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
