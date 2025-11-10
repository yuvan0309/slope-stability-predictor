<script>
  import PredictionForm from './components/PredictionForm.svelte';
  import MultiLayerForm from './components/MultiLayerForm.svelte';
  import ResultsDisplay from './components/ResultsDisplay.svelte';
  import ModelInfo from './components/ModelInfo.svelte';
  
  let predictionResult = null;
  let isLoading = false;
  let selectedModel = 'gradient_boosting';
  let useMultiLayer = true;  // Default to multi-layer mode
  
  function handlePrediction(event) {
    predictionResult = event.detail;
  }
  
  function handleLoading(event) {
    isLoading = event.detail;
  }
  
  function handleModelChange(event) {
    selectedModel = event.detail;
  }
  
  function toggleMode() {
    useMultiLayer = !useMultiLayer;
    predictionResult = null;  // Clear results when switching modes
  }
</script>

<main class="app-container">
  <header class="app-header">
    <div class="header-content">
      <h1>⛰️ Factor of Safety Prediction System</h1>
      <p class="subtitle">Bishop's Simplified Method with Ru (Pore Pressure Ratio)</p>
    </div>
  </header>
  
  <div class="mode-toggle">
    <button 
      class="toggle-btn" 
      class:active={useMultiLayer}
      on:click={toggleMode}
    >
      🪨 Multi-Layer Mode
    </button>
    <button 
      class="toggle-btn" 
      class:active={!useMultiLayer}
      on:click={toggleMode}
    >
      📊 Simple Mode
    </button>
  </div>
  
  <div class="content-wrapper">
    <div class="main-content">
      <section class="prediction-section">
        {#if useMultiLayer}
          <MultiLayerForm 
            bind:selectedModel={selectedModel}
            on:prediction={handlePrediction}
            on:loading={handleLoading}
          />
        {:else}
          <PredictionForm 
            bind:selectedModel={selectedModel}
            on:prediction={handlePrediction}
            on:loading={handleLoading}
            on:modelChange={handleModelChange}
          />
        {/if}
      </section>
      
      {#if predictionResult}
        <section class="results-section">
          <ResultsDisplay result={predictionResult} />
        </section>
      {/if}
    </div>
    
    <aside class="sidebar">
      <ModelInfo model={selectedModel} />
    </aside>
  </div>
  
  <footer class="app-footer">
    <p>Mining ANN Project | Factor of Safety Prediction | November 2025</p>
  </footer>
</main>

<style lang="css">
  .app-container {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
  
  .app-header {
    background: linear-gradient(135deg, #2563eb 0%, #1e40af 100%);
    color: white;
    padding: 2rem 0;
    box-shadow: var(--shadow-lg);
  }
  
  .header-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem;
  }
  
  .app-header h1 {
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: 0.5rem;
  }
  
  .subtitle {
    font-size: 1rem;
    opacity: 0.9;
    font-weight: 400;
  }
  
  .mode-toggle {
    max-width: 1200px;
    margin: 2rem auto 0;
    padding: 0 2rem;
    display: flex;
    gap: 1rem;
    justify-content: center;
  }
  
  .toggle-btn {
    padding: 12px 24px;
    border: 2px solid #e0e0e0;
    background: white;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    color: #64748b;
  }
  
  .toggle-btn:hover {
    border-color: #3498db;
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(52, 152, 219, 0.2);
  }
  
  .toggle-btn.active {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    border-color: #667eea;
    box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
  }
  
  .content-wrapper {
    flex: 1;
    max-width: 1200px;
    margin: 2rem auto;
    padding: 0 2rem;
    width: 100%;
    display: grid;
    grid-template-columns: 1fr 350px;
    gap: 2rem;
  }
  
  .main-content {
    display: flex;
    flex-direction: column;
    gap: 2rem;
  }
  
  .prediction-section,
  .results-section {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 2rem;
    box-shadow: var(--shadow);
    border: 1px solid var(--border-color);
  }
  
  .sidebar {
    position: sticky;
    top: 2rem;
    height: fit-content;
  }
  
  .app-footer {
    background: var(--text-primary);
    color: white;
    text-align: center;
    padding: 1.5rem;
    margin-top: 3rem;
  }
  
  .app-footer p {
    font-size: 0.875rem;
    opacity: 0.8;
  }
  
  @media (max-width: 968px) {
    .content-wrapper {
      grid-template-columns: 1fr;
    }
    
    .sidebar {
      position: static;
    }
    
    .app-header h1 {
      font-size: 1.5rem;
    }
  }
</style>
