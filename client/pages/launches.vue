<template>
    <div>
      <h1>SpaceX Launches</h1>
      <div v-if="loading">Loading...</div>
      <div v-else-if="error">Error fetching data</div>
      <div v-else>
        <div v-for="launch in launches" :key="launch.id">
          <h2>{{ launch.mission_name }}</h2>
          <p><strong>Launch Date:</strong> {{ launch.launch_date }}</p>
          <p><strong>Launch Site:</strong> {{ launch.launch_site.site_name_long }}</p>
          <p><strong>Rocket:</strong> {{ launch.rocket.rocket_name }}</p>
          <p v-if="launch.details"><strong>Details:</strong> {{ launch.details }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { useQuery } from '@vue/apollo-composable';
  import { gql } from 'graphql-tag';
  
  export default {
    setup() {
      const { result, loading, error } = useQuery(gql`
        query GetLaunches {
          launches {
            id
            mission_name
            launch_date
            launch_site {
              site_name_long
            }
            rocket {
              rocket_name
            }
            details
          }
        }
      `);
  
      return {
        loading,
        error,
        launches: result.data ? result.data.launches : [],
      };
    },
  };
  </script>