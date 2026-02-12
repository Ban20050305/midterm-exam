<template>
  <q-page padding>
    <div class="text-h4 q-mb-md">
      Advanced Full-Stack Demo (Quasar + Express)
    </div>

    <!-- Git Workflow -->
    <q-card class="q-mb-md">
      <q-card-section>
        <div class="text-h6">Git Workflow</div>
        <q-list bordered separator class="q-mt-sm">
          <q-item v-for="(step, index) in gitSteps" :key="index">
            <q-item-section avatar>
              <q-badge>{{ index + 1 }}</q-badge>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ step.title }}</q-item-label>
              <q-item-label caption>{{ step.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>

    <!-- Docker Concepts -->
    <q-card class="q-mb-md">
      <q-card-section>
        <div class="text-h6">Docker Concepts</div>
        <q-list bordered separator class="q-mt-sm">
          <q-item v-for="(item, index) in dockerItems" :key="index">
            <q-item-section>
              <q-item-label>{{ item.title }}</q-item-label>
              <q-item-label caption>{{ item.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>

    <!-- New: API Data from Backend -->
    <q-card shadow-lg class="q-mt-md" style="border-radius: 12px; background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);">
      <q-card-section>
        <div class="text-h6 text-primary row items-center">
          <q-icon name="api" class="q-mr-sm" />
          Data from Backend API
        </div>
        <q-separator class="q-my-sm" />
        
        <div v-if="loading" class="flex flex-center q-pa-lg">
          <q-spinner-dots color="primary" size="40px" />
        </div>
        
        <q-list v-else bordered separator class="bg-white" style="border-radius: 8px;">
          <q-item v-if="apiData.git" class="q-py-md">
            <q-item-section avatar>
              <q-icon name="code" color="secondary" />
            </q-item-section>
            <q-item-section>
              <q-item-label class="text-weight-bold">Advanced Git</q-item-label>
              <q-item-label caption>{{ apiData.git.detail }}</q-item-label>
            </q-item-section>
          </q-item>

          <q-item v-if="apiData.docker" class="q-py-md">
            <q-item-section avatar>
              <q-icon name="layers" color="secondary" />
            </q-item-section>
            <q-item-section>
              <q-item-label class="text-weight-bold">Advanced Docker</q-item-label>
              <q-item-label caption>{{ apiData.docker.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>

        <div class="flex justify-end q-mt-md">
          <q-btn 
            v-if="!loading" 
            color="primary" 
            label="Refresh Data" 
            icon="refresh"
            rounded
            unelevated
            @click="fetchData" 
          />
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const gitSteps = [
  { title: 'Initialization', detail: 'Initialize git repository and connect to remote origin.' },
  { title: 'Backend Init', detail: 'Initialize express backend with core dependencies.' },
  { title: 'Backend API', detail: 'Implement /api/demo endpoint with logging and error handling.' }
];

const dockerItems = [
  { title: 'Dockerfile', detail: 'Multi-stage build for optimized production images.' },
  { title: 'Docker Compose', detail: 'Orchestrate fullstack application with network and volumes.' }
];

const apiData = ref({ git: {}, docker: {} });
const loading = ref(true);

const fetchData = async () => {
  loading.value = true;
  try {
    const apiUrl = import.meta.env.VITE_API_URL || 'http://localhost:3000';
    const response = await axios.get(apiUrl + '/api/demo');
    apiData.value = response.data;
  } catch (error) {
    console.error('API Error:', error);
  } finally {
    loading.value = false;
  }
};

onMounted(fetchData);
</script>
