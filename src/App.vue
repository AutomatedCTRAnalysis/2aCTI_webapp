<template>
  <div id="app" class="container">
  
  <center><div id = "logo">
    <img src="./assets/2aCTI.png">
  </div></center>


  <h1 class="title mt-0">Automated Analysis of Cyber Threat Intelligence</h1>

   <b-message 
      title="Welcome!" 
      type="is-warning" 
      aria-close-label="Close message">
      This system is built to retrieve and predict Tactics and Techniques (TTPs) according to MITRE's ATTACK framework.
      Using web-scraping techniques and Machine Learning classifiers, 2aCTI allows users to search for TTPs used in passed attacks
      or predict their presence in CTI reports.
    </b-message>


  
  <h2 class="mx-6">Enter the name of a malware to retrieve the Tactics and Techniques used according to MITRE's database  </h2>
  <br><br>

  <div class="searchBar">
    <b-input placeholder="Search..." rounded v-model="searchBar"></b-input>
  </div>
  <br>
  <b-button type="is-link is-light" rounded @click="submitSearchForm">Search!</b-button><br />

  <br>
  <h2> Or enter directly a CTI report to predict the corresponding TTPs</h2>
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

  <b-button type="is-link is-light" rounded @click="submitTextForm">Predict!</b-button><br />
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
          label: '5) Persistence (TA0003)',
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
          label: '6) Privilege Escalation (TA0004)',
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
          label: '7) Defense Evasion (TA0005)',
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
          label: '8) Credential Access (TA0006)',
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
              label: 'Network Service Discovery',
              description: 'Adversaries may attempt to get a listing of services running on remote hosts and local network infrastructure devices, including those that may be vulnerable to remote software exploitation. Common methods to acquire this information include port and/or vulnerability scans using tools that are brought onto a system.'
            },
            {
              id: 'T1087', 
              label: 'Account Discovery', 
              description: 'Adversaries may attempt to get a listing of accounts on a system or within an environment. This information can help adversaries determine which accounts exist to aid in follow-on behavior.'
            },
            {
              id: 'T1010',
              label: 'Application Window Discovery',
              description: 'Adversaries may attempt to get a listing of open application windows. Window listings could convey information about how the system is used or give context to information collected by a keylogger.'
            },
            {
              id: 'T1217',
              label: 'Browser Bookmark Discovery',
              description: 'Adversaries may enumerate browser bookmarks to learn more about compromised hosts. Browser bookmarks may reveal personal information about users (ex: banking sites, interests, social media, etc.) as well as details about internal network resources such as servers, tools/dashboards, or other related infrastructure.'
            },
            {
              id: 'T1580', 
              label: 'Cloud Infrastructure Discovery',
              description: 'An adversary may attempt to discover infrastructure and resources that are available within an infrastructure-as-a-service (IaaS) environment. This includes compute service resources such as instances, virtual machines, and snapshots as well as resources of other services including the storage and database services.'
            },
            {
              id: 'T1538',
              label: 'Cloud Service Dashboard',
              description: 'An adversary may use a cloud service dashboard GUI with stolen credentials to gain useful information from an operational cloud environment, such as specific services, resources, and features. For example, the GCP Command Center can be used to view all assets, findings of potential security risks, and to run additional queries, such as finding public IP addresses and open ports.'
            },
            {
              id: 'T1526',
              label: 'Cloud Service Discovery',
              description: 'An adversary may attempt to enumerate the cloud services running on a system after gaining access. These methods can differ from platform-as-a-service (PaaS), to infrastructure-as-a-service (IaaS), or software-as-a-service (SaaS). Many services exist throughout the various cloud providers and can include Continuous Integration and Continuous Delivery (CI/CD), Lambda Functions, Azure AD, etc.'
            },
            {
              id: 'T1619', 
              label: 'Cloud Storage Object Discovery',
              description: 'Adversaries may enumerate objects in cloud storage infrastructure. Adversaries may use this information during automated discovery to shape follow-on behaviors, including requesting all or specific objects from cloud storage. Similar to File and Directory Discovery on a local host, after identifying available storage services (i.e. Cloud Infrastructure Discovery) adversaries may access the contents/objects stored in cloud infrastructure.'
            },
            {
              id: 'T1613',
              label: 'Container and Resource Discovery',
              description: 'Adversaries may attempt to discover containers and other resources that are available within a containers environment. Other resources may include images, deployments, pods, nodes, and other information such as the status of a cluster.'
            },
            {
              id: 'T1622', 
              label: 'Debugger Evasion',
              description: '	Adversaries may employ various means to detect and avoid debuggers. Debuggers are typically used by defenders to trace and/or analyze the execution of potential malware payloads.'
            },
            {
              id: 'T1482', 
              label: 'Domain Trust Discovery',
              description: 'Adversaries may attempt to gather information on domain trust relationships that may be used to identify lateral movement opportunities in Windows multi-domain/forest environments. Domain trusts provide a mechanism for a domain to allow access to resources based on the authentication procedures of another domain. Domain trusts allow the users of the trusted domain to access resources in the trusting domain. The information discovered may help the adversary conduct SID-History Injection, Pass the Ticket, and Kerberoasting. Domain trusts can be enumerated using the DSEnumerateDomainTrusts() Win32 API call, .NET methods, and LDAP. The Windows utility Nltest is known to be used by adversaries to enumerate domain trusts.'
            },
            {
              id: 'T1083',
              label: 'File and Directory Discovery',
              description: 'Adversaries may enumerate files and directories or may search in specific locations of a host or network share for certain information within a file system. Adversaries may use the information from File and Directory Discovery during automated discovery to shape follow-on behaviors, including whether or not the adversary fully infects the target and/or attempts specific actions.'
            },
            {
              id: 'T1615', 
              label: 'Group Policy Discovery',
              description: 'Adversaries may gather information on Group Policy settings to identify paths for privilege escalation, security measures applied within a domain, and to discover patterns in domain objects that can be manipulated or used to blend in the environment. Group Policy allows for centralized management of user and computer settings in Active Directory (AD). Group policy objects (GPOs) are containers for group policy settings made up of files stored within a predicable network path \\SYSVOL\\Policies\.'
            },
            {
              id: 'T1135', 
              label: 'Network Share Discovery',
              description: 'Adversaries may look for folders and drives shared on remote systems as a means of identifying sources of information to gather as a precursor for Collection and to identify potential systems of interest for Lateral Movement. Networks often contain shared network drives and folders that enable users to access file directories on various systems across a network.'
            },
            {
              id: 'T1201', 
              label: 'Password Policy Discovery',
              description: 'Adversaries may attempt to access detailed information about the password policy used within an enterprise network or cloud environment. Password policies are a way to enforce complex passwords that are difficult to guess or crack through Brute Force. This information may help the adversary to create a list of common passwords and launch dictionary and/or brute force attacks which adheres to the policy (e.g. if the minimum password length should be 8, then not trying passwords not checking for more than 3-4 passwords per account if the lockout is set to 6 as to not lock out accounts).'
            },
            {
              id: 'T1120',
              label: 'Peripheral Device Discovery',
              description: 'Adversaries may attempt to gather information about attached peripheral devices and components connected to a computer system. Peripheral devices could include auxiliary resources that support a variety of functionalities such as keyboards, printers, cameras, smart card readers, or removable storage. The information may be used to enhance their awareness of the system and network environment or may be used for further actions.'
            },
            {
              id: 'T1069', 
              label: 'Permission Groups Discovery',
              description: 'Adversaries may attempt to find group and permission settings. This information can help adversaries determine which user accounts and groups are available, the membership of users in particular groups, and which users and groups have elevated permissions.'
            },
            {
              id: 'T1057', 
              label: 'Process Discovery',
              description: 'Adversaries may attempt to get information about running processes on a system. Information obtained could be used to gain an understanding of common software/applications running on systems within the network. Adversaries may use the information from Process Discovery during automated discovery to shape follow-on behaviors, including whether or not the adversary fully infects the target and/or attempts specific actions.'
            },
            {
              id: 'T1012',
              label: 'Query Registry',
              description: 'Adversaries may interact with the Windows Registry to gather information about the system, configuration, and installed software.'
            },
            {
              id: 'T1018', 
              label: 'Remote System Discovery',
              description: 'Adversaries may attempt to get a listing of other systems by IP address, hostname, or other logical identifier on a network that may be used for Lateral Movement from the current system. Functionality could exist within remote access tools to enable this, but utilities available on the operating system could also be used such as Ping or net view using Net.'
            },
            {
              id: 'T1518', 
              label: 'Software Discovery',
              description: 'Adversaries may attempt to get a listing of software and software versions that are installed on a system or in a cloud environment. Adversaries may use the information from Software Discovery during automated discovery to shape follow-on behaviors, including whether or not the adversary fully infects the target and/or attempts specific actions.'
            },
            {
              id: 'T1082', 
              label: 'System Information Discovery',
              description: 'An adversary may attempt to get detailed information about the operating system and hardware, including version, patches, hotfixes, service packs, and architecture. Adversaries may use the information from System Information Discovery during automated discovery to shape follow-on behaviors, including whether or not the adversary fully infects the target and/or attempts specific actions.'
            },
            {
              id: 'T1614',
              label: 'System Location Discovery',
              description: 'Adversaries may gather information in an attempt to calculate the geographical location of a victim host. Adversaries may use the information from System Location Discovery during automated discovery to shape follow-on behaviors, including whether or not the adversary fully infects the target and/or attempts specific actions.'
            },
            {
              id: 'T1016', 
              label: 'System Network Configuration Discovery',
              description: 'Adversaries may look for details about the network configuration and settings, such as IP and/or MAC addresses, of systems they access or through information discovery of remote systems. Several operating system administration utilities exist that can be used to gather this information. Examples include Arp, ipconfig/ifconfig, nbtstat, and route.'
            },
            {
              id: 'T1049',
              label: 'System Network Connections Discovery',
              description: 'Adversaries may attempt to get a listing of network connections to or from the compromised system they are currently accessing or from remote systems by querying for information over the network.'
            },
            {
              id: 'T1033', 
              label: 'System Owner/User Discovery',
              description: 'Adversaries may attempt to identify the primary user, currently logged in user, set of users that commonly uses a system, or whether a user is actively using the system. They may do this, for example, by retrieving account usernames or by using OS Credential Dumping. The information may be collected in a number of different ways using other Discovery techniques, because user and username details are prevalent throughout a system and include running process ownership, file/directory ownership, session information, and system logs. Adversaries may use the information from System Owner/User Discovery during automated discovery to shape follow-on behaviors, including whether or not the adversary fully infects the target and/or attempts specific actions.'
            },
            {
              id: 'T1007', 
              label: 'System Service Discovery', 
              description: 'Adversaries may try to gather information about registered local system services. Adversaries may obtain information about services using tools as well as OS utility commands such as sc query, tasklist /svc, systemctl --type=service, and net start.'
            },
            {
              id: 'T1124', 
              label: 'System Time Discovery',
              description: 'An adversary may gather the system time and/or time zone from a local or remote system. The system time is set and stored by the Windows Time Service within a domain to maintain time synchronization between systems and services in an enterprise network.'
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
              label: 'Exploitation of Remote Services',
              description: 'Adversaries may exploit remote services to gain unauthorized access to internal systems once inside of a network. Exploitation of a software vulnerability occurs when an adversary takes advantage of a programming error in a program, service, or within the operating system software or kernel itself to execute adversary-controlled code. A common goal for post-compromise exploitation of remote services is for lateral movement to enable access to a remote system.'
            },
            {
              id: 'T1534',
              label: 'Internal Spearphishing',
              description: 'Adversaries may use internal spearphishing to gain access to additional information or exploit other users within the same organization after they already have access to accounts or systems within the environment. Internal spearphishing is multi-staged campaign where an email account is owned either by controlling the user\'s device with previously installed malware or by compromising the account credentials of the user. Adversaries attempt to take advantage of a trusted internal account to increase the likelihood of tricking the target into falling for the phish attempt.'
            },
            {
              id: 'T1570',
              label: 'Lateral Tool Transfer',
              description: 'Adversaries may transfer tools or other files between systems in a compromised environment. Once brought into the victim environment (i.e. Ingress Tool Transfer) files may then be copied from one system to another to stage adversary tools or other files over the course of an operation. Adversaries may copy files between internal victim systems to support lateral movement using inherent file sharing protocols such as file sharing over SMB/Windows Admin Shares to connected network shares or with authenticated connections via Remote Desktop Protocol.'
            },
            {
              id: 'T1563', 
              label: 'Remote Service Session Hijacking',
              description: 'Adversaries may take control of preexisting sessions with remote services to move laterally in an environment. Users may use valid credentials to log into a service specifically designed to accept remote connections, such as telnet, SSH, and RDP. When a user logs into a service, a session will be established that will allow them to maintain a continuous interaction with that service.'
            },
            {
              id: 'T1021',
              label: 'Remote Services',
              description: 'Adversaries may use Valid Accounts to log into a service specifically designed to accept remote connections, such as telnet, SSH, and VNC. The adversary may then perform actions as the logged-on user.'
            },
            {
              id: 'T1091',
              label: 'Replication Through Removable Media',
              description: 'Adversaries may move onto systems, possibly those on disconnected or air-gapped networks, by copying malware to removable media and taking advantage of Autorun features when the media is inserted into a system and executes. In the case of Lateral Movement, this may occur through modification of executable files stored on removable media or by copying malware and renaming it to look like a legitimate file to trick users into executing it on a separate system. In the case of Initial Access, this may occur through manual manipulation of the media, modification of systems used to initially format the media, or modification to the media\'s firmware itself.'
            },
            {
              id: 'T1072', 
              label: 'Software Deployment Tools', 
              description: 'Adversaries may gain access to and use third-party software suites installed within an enterprise network, such as administration, monitoring, and deployment systems, to move laterally through the network. Third-party applications and software deployment systems may be in use in the network environment for administration purposes (e.g., SCCM, HBSS, Altiris, etc.).'
            },
            {
              id: 'T1080',
              label: 'Taint Shared Content',
              description: 'Adversaries may deliver payloads to remote systems by adding content to shared storage locations, such as network drives or internal code repositories. Content stored on network drives or in other shared locations may be tainted by adding malicious programs, scripts, or exploit code to otherwise valid files. Once a user opens the shared tainted content, the malicious portion can be executed to run the adversary\'s code on a remote system. Adversaries may use tainted shared content to move laterally.'
            },
            {
              id: 'T1550', 
              label: 'Use Alternate Authentication Material',
              description: 'Adversaries may use alternate authentication material, such as password hashes, Kerberos tickets, and application access tokens, in order to move laterally within an environment and bypass normal system access controls.'
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
              label: 'Archive Collected Data',
              description: 'An adversary may compress and/or encrypt data that is collected prior to exfiltration. Compressing the data can help to obfuscate the collected data and minimize the amount of data sent over the network. Encryption can be used to hide information that is being exfiltrated from detection or make exfiltration less conspicuous upon inspection by a defender.'
            },
            {
              id: 'T1123',
              label: 'Audio Capture',
              description: 'An adversary can leverage a computer\'s peripheral devices (e.g., microphones and webcams) or applications (e.g., voice and video call services) to capture audio recordings for the purpose of listening into sensitive conversations to gather information.'
            },
            {
              id: 'T1119', 
              label: 'Automated Collection',
              description: 'Once established within a system or network, an adversary may use automated techniques for collecting internal data. Methods for performing this technique could include use of a Command and Scripting Interpreter to search for and copy information fitting set criteria such as file type, location, or name at specific time intervals. In cloud-based environments, adversaries may also use cloud APIs, command line interfaces, or extract, transform, and load (ETL) services to automatically collect data. This functionality could also be built into remote access tools.'
            },
            {
              id: 'T1185',
              label: 'Browser Session Hijacking',
              description: 'Adversaries may take advantage of security vulnerabilities and inherent functionality in browser software to change content, modify user-behaviors, and intercept information as part of various browser session hijacking techniques.'
            },
            {
              id: 'T1115',
              label: 'Clipboard Data',
              description: 'Adversaries may collect data stored in the clipboard from users copying information within or between applications.'
            },
            {
              id: 'T1530', 
              label: 'Data from Cloud Storage Object',
              description: 'Adversaries may access data objects from improperly secured cloud storage.'
            },
            {
              id: 'T1602',
              label: 'Data from Configuration Repository',
              description: 'Adversaries may collect data related to managed devices from configuration repositories. Configuration repositories are used by management systems in order to configure, manage, and control data on remote systems. Configuration repositories may also facilitate remote access and administration of devices.'
            },
            {
              id: 'T1213', 
              label: 'Data from Information Repositories',
              description: 'Adversaries may leverage information repositories to mine valuable information. Information repositories are tools that allow for storage of information, typically to facilitate collaboration or information sharing between users, and can store a wide variety of data that may aid adversaries in further objectives, or direct access to the target information. Adversaries may also abuse external sharing features to share sensitive documents with recipients outside of the organization.'
            },
            {
              id: 'T1005',
              label: 'Data from Local System',
              description: 'Adversaries may search local system sources, such as file systems and configuration files or local databases, to find files of interest and sensitive data prior to Exfiltration.'
            },
            {
              id: 'T1039',
              label: 'Data from Network Shared Drive',
              description: 'Adversaries may search network shares on computers they have compromised to find files of interest. Sensitive data can be collected from remote systems via shared network drives (host shared directory, network file server, etc.) that are accessible from the current system prior to Exfiltration. Interactive command shells may be in use, and common functionality within cmd may be used to gather information.'
            },
            {
              id: 'T1025', 
              label: 'Data from Removable Media',
              description: 'Adversaries may search connected removable media on computers they have compromised to find files of interest. Sensitive data can be collected from any removable media (optical disk drive, USB memory, etc.) connected to the compromised system prior to Exfiltration. Interactive command shells may be in use, and common functionality within cmd may be used to gather information.'
            },
            {
              id: 'T1074',
              label: 'Data Staged',
              description: 'Adversaries may stage collected data in a central location or directory prior to Exfiltration. Data may be kept in separate files or combined into one file through techniques such as Archive Collected Data. Interactive command shells may be used, and common functionality within cmd and bash may be used to copy data into a staging location.'
            },
            {
              id: 'T1114', 
              label: 'Email Collection',
              description: '	Adversaries may target user email to collect sensitive information. Emails may contain sensitive data, including trade secrets or personal information, that can prove valuable to adversaries. Adversaries can collect or forward email from mail servers or clients'
            },
            {
              id: 'T1056',
              label: 'Input Capture',
              description: 'Adversaries may forge credential materials that can be used to gain access to web applications or Internet services. Web applications and services (hosted in cloud SaaS environments or on-premise servers) often use session cookies, tokens, or other materials to authenticate and authorize user access.'
            },
            {
              id: 'T1113',
              label: 'Screen Capture',
              description: 'Adversaries may attempt to take screen captures of the desktop to gather information over the course of an operation. Screen capturing functionality may be included as a feature of a remote access tool used in post-compromise operations. Taking a screenshot is also typically possible through native utilities or API calls, such as CopyFromScreen, xwd, or screencapture.'
            },
            {
              id: 'T1125', 
              label: 'Video Capture',
              description: 'An adversary can leverage a computer\'s peripheral devices (e.g., integrated cameras or webcams) or applications (e.g., video call services) to capture video recordings for the purpose of gathering information. Images may also be captured from devices or applications, potentially in specified intervals, in lieu of video files.'
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
              label: 'Application Layer Protocol',
              description: 'Adversaries may communicate using application layer protocols to avoid detection/network filtering by blending in with existing traffic. Commands to the remote system, and often the results of those commands, will be embedded within the protocol traffic between the client and server.'
            },
            {
              id: 'T1092',
              label: 'Communication Through Removable Media',
              description: 'Adversaries can perform command and control between compromised hosts on potentially disconnected networks using removable media to transfer commands from system to system. Both systems would need to be compromised, with the likelihood that an Internet-connected system was compromised first and the second through lateral movement by Replication Through Removable Media. Commands and files would be relayed from the disconnected system to the Internet-connected system to which the adversary has direct access.'
            },
            {
              id: 'T1132',
              label: 'Data Encoding',
              description: 'Adversaries may encode data to make the content of command and control traffic more difficult to detect. Command and control (C2) information can be encoded using a standard data encoding system. Use of data encoding may adhere to existing protocol specifications and includes use of ASCII, Unicode, Base64, MIME, or other binary-to-text and character encoding systems. Some data encoding systems may also result in data compression, such as gzip.'
            },
            {
              id: 'T1001', 
              label: 'Data Obfuscation',
              description: 'Adversaries may obfuscate command and control traffic to make it more difficult to detect. Command and control (C2) communications are hidden (but not necessarily encrypted) in an attempt to make the content more difficult to discover or decipher and to make the communication less conspicuous and hide commands from being seen. This encompasses many methods, such as adding junk data to protocol traffic, using steganography, or impersonating legitimate protocols.'
            },
            {
              id: 'T1568',
              label: 'Dynamic Resolution',
              description: 'Adversaries may dynamically establish connections to command and control infrastructure to evade common detections and remediations. This may be achieved by using malware that shares a common algorithm with the infrastructure the adversary uses to receive the malware\'s communications. These calculations can be used to dynamically adjust parameters such as the domain name, IP address, or port number the malware uses for command and control.'
            },
            {
              id: 'T1573',
              label: 'Encrypted Channel',
              description: 'Adversaries may employ a known encryption algorithm to conceal command and control traffic rather than relying on any inherent protections provided by a communication protocol. Despite the use of a secure algorithm, these implementations may be vulnerable to reverse engineering if secret keys are encoded and/or generated within malware samples/configuration files.'
            },
            {
              id: 'T1008', 
              label: 'Fallback Channels',
              description: 'Adversaries may use fallback or alternate communication channels if the primary channel is compromised or inaccessible in order to maintain reliable command and control and to avoid data transfer thresholds.'
            },
            {
              id: 'T1105',
              label: 'Ingress Tool Transfer',
              description: 'Adversaries may transfer tools or other files from an external system into a compromised environment. Tools or files may be copied from an external adversary-controlled system to the victim network through the command and control channel or through alternate protocols such as ftp. Once present, adversaries may also transfer/spread tools between victim devices within a compromised environment (i.e. Lateral Tool Transfer).'
            },
            {
              id: 'T1104', 
              label: 'Multi-Stage Channels',
              description: 'Adversaries may create multiple stages for command and control that are employed under different conditions or for certain functions. Use of multiple stages may obfuscate the command and control channel to make detection more difficult.'
            },
            {
              id: 'T1095',
              label: 'Non-Application Layer Protocol',
              description:'Adversaries may use a non-application layer protocol for communication between host and C2 server or among infected hosts within a network. The list of possible protocols is extensive. Specific examples include use of network layer protocols, such as the Internet Control Message Protocol (ICMP), transport layer protocols, such as the User Datagram Protocol (UDP), session layer protocols, such as Socket Secure (SOCKS), as well as redirected/tunneled protocols, such as Serial over LAN (SOL).'
            },
            {
              id: 'T1571',
              label: 'Non-Standard Port',
              description: 'Adversaries may communicate using a protocol and port paring that are typically not associated. For example, HTTPS over port 8088 or port 587 as opposed to the traditional port 443. Adversaries may make changes to the standard port used by a protocol to bypass filtering or muddle analysis/parsing of network data.'
            },
            {
              id: 'T1572', 
              label: 'Protocol Tunnelin',
              description: 'Adversaries may tunnel network communications to and from a victim system within a separate protocol to avoid detection/network filtering and/or enable access to otherwise unreachable systems. Tunneling involves explicitly encapsulating a protocol within another. This behavior may conceal malicious traffic by blending in with existing traffic and/or provide an outer layer of encryption (similar to a VPN). Tunneling could also enable routing of network packets that would otherwise not reach their intended destination, such as SMB, RDP, or other traffic that would be filtered by network appliances or not routed over the Internet.'
            },
            {
              id: 'T1090',
              label: 'Proxy',
              description: 'Adversaries may use a connection proxy to direct network traffic between systems or act as an intermediary for network communications to a command and control server to avoid direct connections to their infrastructure. Many tools exist that enable traffic redirection through proxies or port redirection, including HTRAN, ZXProxy, and ZXPortMap. Adversaries use these types of proxies to manage command and control communications, reduce the number of simultaneous outbound network connections, provide resiliency in the face of connection loss, or to ride over existing trusted communications paths between victims to avoid suspicion. Adversaries may chain together multiple proxies to further disguise the source of malicious traffic.'
            },
            {
              id: 'T1219', 
              label: 'Remote Access Software',
              description: 'An adversary may use legitimate desktop support and remote access software, such as Team Viewer, AnyDesk, Go2Assist, LogMein, AmmyyAdmin, etc, to establish an interactive command and control channel to target systems within networks. These services are commonly used as legitimate technical support software, and may be allowed by application control within a target environment. Remote access tools like VNC, Ammyy, and Teamviewer are used frequently when compared with other legitimate software commonly used by adversaries.'
            },
            {
              id: 'T1205',
              label: 'Traffic Signaling',
              description: 'Adversaries may use traffic signaling to hide open ports or other malicious functionality used for persistence or command and control. Traffic signaling involves the use of a magic value or sequence that must be sent to a system to trigger a special response, such as opening a closed port or executing a malicious task. This may take the form of sending a series of packets with certain characteristics before a port will be opened that the adversary can use for command and control. Usually this series of packets consists of attempted connections to a predefined sequence of closed ports (i.e. Port Knocking), but can involve unusual flags, specific strings, or other unique characteristics. After the sequence is completed, opening a port may be accomplished by the host-based firewall, but could also be implemented by custom software.'
            },
            {
              id: 'T1102',
              label: 'Web Service',
              description: 'Adversaries may use an existing, legitimate external Web service as a means for relaying data to/from a compromised system. Popular websites and social media acting as a mechanism for C2 may give a significant amount of cover due to the likelihood that hosts within a network are already communicating with them prior to a compromise. Using common services, such as those offered by Google or Twitter, makes it easier for adversaries to hide in expected noise. Web service providers commonly use SSL/TLS encryption, giving adversaries an added level of protection.'
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
              label: 'Automated Exfiltration',
              description: 'Adversaries may exfiltrate data, such as sensitive documents, through the use of automated processing after being gathered during Collection.'
            },
            {
              id: 'T1030',
              label: 'Data Transfer Size Limits',
              description: 'An adversary may exfiltrate data in fixed size chunks instead of whole files or limit packet sizes below certain thresholds. This approach may be used to avoid triggering network data transfer threshold alerts.'
            },
            {
              id: 'T1048',
              label: 'Exfiltration Over Alternative Protocol',
              description: 'Adversaries may steal data by exfiltrating it over a different protocol than that of the existing command and control channel. The data may also be sent to an alternate network location from the main command and control server.'
            },
            {
              id: 'T1041', 
              label: 'Exfiltration Over C2 Channel',
              description: 'Adversaries may steal data by exfiltrating it over an existing command and control channel. Stolen data is encoded into the normal communications channel using the same protocol as command and control communications.'
            },
            {
              id: 'T1011',
              label: 'Exfiltration Over Other Network Medium',
              description: 'Adversaries may attempt to exfiltrate data over a different network medium than the command and control channel. If the command and control network is a wired Internet connection, the exfiltration may occur, for example, over a WiFi connection, modem, cellular data connection, Bluetooth, or another radio frequency (RF) channel.'
            },
            {
              id: 'T1052',
              label: 'Exfiltration Over Physical Medium',
              description: 'Adversaries may attempt to exfiltrate data via a physical medium, such as a removable drive. In certain circumstances, such as an air-gapped network compromise, exfiltration could occur via a physical medium or device introduced by a user. Such media could be an external hard drive, USB drive, cellular phone, MP3 player, or other removable storage and processing device. The physical medium or device could be used as the final exfiltration point or to hop between otherwise disconnected systems.'
            },
            {
              id: 'T1567', 
              label: 'Exfiltration Over Web Service',
              description: 'Adversaries may use an existing, legitimate external Web service to exfiltrate data rather than their primary command and control channel. Popular Web services acting as an exfiltration mechanism may give a significant amount of cover due to the likelihood that hosts within a network are already communicating with them prior to compromise. Firewall rules may also already exist to permit traffic to these services.'
            },
            {
              id: 'T1029',
              label: 'Scheduled Transfer',
              description: 'Adversaries may schedule data exfiltration to be performed only at certain times of day or at certain intervals. This could be done to blend traffic patterns with normal activity or availability.'
            },
            {
              id: 'T1537', 
              label: 'Transfer Data to Cloud Account',
              description: 'Adversaries may exfiltrate data by transferring the data, including backups of cloud environments, to another cloud account they control on the same service to avoid typical file transfers/downloads and network-based exfiltration detection.'
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
              label: 'Account Access Removal',
              description: 'Adversaries may interrupt availability of system and network resources by inhibiting access to accounts utilized by legitimate users. Accounts may be deleted, locked, or manipulated (ex: changed credentials) to remove access to accounts. Adversaries may also subsequently log off and/or perform a System Shutdown/Reboot to set malicious changes into place.'
            },
            {
              id: 'T1485',
              label: 'Data Destruction',
              description: 'Adversaries may destroy data and files on specific systems or in large numbers on a network to interrupt availability to systems, services, and network resources. Data destruction is likely to render stored data irrecoverable by forensic techniques through overwriting files or data on local and remote drives. Common operating system file deletion commands such as del and rm often only remove pointers to files without wiping the contents of the files themselves, making the files recoverable by proper forensic methodology. This behavior is distinct from Disk Content Wipe and Disk Structure Wipe because individual files are destroyed rather than sections of a storage disk or the disk\'s logical structure.'
            },
            {
              id: 'T1486',
              label: 'Data Encrypted for Impact',
              description: 'Adversaries may encrypt data on target systems or on large numbers of systems in a network to interrupt availability to system and network resources. They can attempt to render stored data inaccessible by encrypting files or data on local and remote drives and withholding access to a decryption key. This may be done in order to extract monetary compensation from a victim in exchange for decryption or a decryption key (ransomware) or to render data permanently inaccessible in cases where the key is not saved or transmitted.'
            },
            {
              id: 'T1565', 
              label: 'Data Manipulation',
              description: 'Adversaries may insert, delete, or manipulate data in order to influence external outcomes or hide activity, thus threatening the integrity of the data. By manipulating data, adversaries may attempt to affect a business process, organizational understanding, or decision making.'
            },
            {
              id: 'T1491',
              label: 'Defacement',
              description: 'Adversaries may modify visual content available internally or externally to an enterprise network, thus affecting the integrity of the original content. Reasons for Defacement include delivering messaging, intimidation, or claiming (possibly false) credit for an intrusion. Disturbing or offensive images may be used as a part of Defacement in order to cause user discomfort, or to pressure compliance with accompanying messages.'
            },
            {
              id: 'T1561',
              label: 'Disk Wipe',
              description: 'Adversaries may wipe or corrupt raw disk data on specific systems or in large numbers in a network to interrupt availability to system and network resources. With direct write access to a disk, adversaries may attempt to overwrite portions of disk data. Adversaries may opt to wipe arbitrary portions of disk data and/or wipe disk structures like the master boot record (MBR). A complete wipe of all disk sectors may be attempted.'
            },
            {
              id: 'T1499', 
              label: 'Endpoint Denial of Service',
              description: 'Adversaries may perform Endpoint Denial of Service (DoS) attacks to degrade or block the availability of services to users. Endpoint DoS can be performed by exhausting the system resources those services are hosted on or exploiting the system to cause a persistent crash condition. Example services include websites, email services, DNS, and web-based applications. Adversaries have been observed conducting DoS attacks for political purposes and to support other malicious activities, including distraction, hacktivism, and extortion.'
            },
            {
              id: 'T1495',
              label: 'Firmware Corruption',
              description: 'Adversaries may overwrite or corrupt the flash memory contents of system BIOS or other firmware in devices attached to a system in order to render them inoperable or unable to boot, thus denying the availability to use the devices and/or the system. Firmware is software that is loaded and executed from non-volatile memory on hardware devices in order to initialize and manage device functionality. These devices could include the motherboard, hard drive, or video cards.'
            },
            {
              id: 'T1490', 
              label: 'Inhibit System Recovery',
              description: 'Adversaries may delete or remove built-in operating system data and turn off services designed to aid in the recovery of a corrupted system to prevent recovery. This may deny access to available backups and recovery options.'
            },
            {
              id: 'T1498',
              label: 'Network Denial of Service',
              description: 'Adversaries may perform Network Denial of Service (DoS) attacks to degrade or block the availability of targeted resources to users. Network DoS can be performed by exhausting the network bandwidth services rely on. Example resources include specific websites, email services, DNS, and web-based applications. Adversaries have been observed conducting network DoS attacks for political purposes and to support other malicious activities, including distraction, hacktivism, and extortion.'
            },
            {
              id: 'T1496',
              label: 'Resource Hijacking',
              description: 'Adversaries may leverage the resources of co-opted systems in order to solve resource intensive problems, which may impact system and/or hosted service availability.'
            },
            {
              id: 'T1489', 
              label: 'Service Stop',
              description: 'Adversaries may stop or disable services on a system to render those services unavailable to legitimate users. Stopping critical services or processes can inhibit or stop response to an incident or aid in the \'s overall objectives to cause damage to the environment.'
            },
            {
              id: 'T1529',
              label: 'System Shutdown/Reboot',
              description: 'Adversaries may shutdown/reboot systems to interrupt access to, or aid in the destruction of, those systems. Operating systems may contain commands to initiate a shutdown/reboot of a machine or network device. In some cases, these commands may also be used to initiate a shutdown/reboot of a remote computer or network device. Shutting down or rebooting systems may disrupt access to computer resources for legitimate users.'
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
      api_url: "https://api-cti.herokuapp.com/classification", // API adress for classification 
      search_url: "https://api-cti.herokuapp.com/searchattack", // API adress for search bar
      display_boxes: false,
      search_display_boxes: false,
    }
  },
  methods: { // retrieve response for search bar
    async submitSearchForm() {
      this.search_display_boxes = false
      this.display_boxes = false
      this.isLoading = true
      let response = await axios.post(this.search_url, {'sentence': this.searchBar}) // call API
      .then(response => {
        if(response.data.status === 'found'){
          this.techniques = response.data.techniques
          this.description = response.data.description
          this.search_display_boxes = true
          this.display_boxes = false
          this.isLoading = false
        }
        else{
          this.search_display_boxes = true
          this.description = "Sorry, at the moment the attack you searched for cannot be found in the MITRE ATT\&CK database. Please try pasting a report below to predict which TTPs were used."
          this.isLoading = false
        }
      }) 
    },
    async submitTextForm(){ // retrieve response for text classification
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
      }) 
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
  height: 220px;
  width: 330px; 
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
