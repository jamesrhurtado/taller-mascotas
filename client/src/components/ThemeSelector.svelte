<script lang="ts">
    // List of available themes
    const themes = [
        { id: 'default', name: 'Default', icon: '🌟' },
        { id: 'eighties-retro', name: '80s Retro', icon: '🕹️' },
        { id: 'terminal-classic', name: 'Terminal Classic', icon: '💻' },
        { id: 'hand-sketched', name: 'Hand-Sketched', icon: '✏️' },
        { id: 'steampunk', name: 'Steampunk', icon: '⚙️' },
        { id: 'fantasy-realm', name: 'Fantasy Realm', icon: '🧙' }
    ];

    let currentTheme = 'default';

    // Function to apply the selected theme
    function applyTheme(themeId: string) {
        // Remove all previous theme classes
        document.documentElement.classList.remove(
            'default', 'eighties-retro', 'terminal-classic', 'hand-sketched', 'steampunk', 'fantasy-realm'
        );
        
        // Add the new theme class
        document.documentElement.classList.add(themeId);
        
        // Store the user's theme preference
        localStorage.setItem('tailspin-theme', themeId);
        
        currentTheme = themeId;
    }

    // Function to initialize the theme from local storage on page load
    function initializeTheme() {
        const storedTheme = localStorage.getItem('tailspin-theme');
        if (storedTheme) {
            currentTheme = storedTheme;
            applyTheme(storedTheme);
        }
    }

    // Execute on component mount
    import { onMount } from 'svelte';
    onMount(() => {
        initializeTheme();
    });
</script>

<div class="theme-selector">
    <div class="dropdown">
        <button class="theme-button" aria-label="Select a theme">
            <span class="theme-icon">{themes.find(t => t.id === currentTheme)?.icon || '🌟'}</span>
            <span class="theme-name">{themes.find(t => t.id === currentTheme)?.name || 'Default'}</span>
            <svg xmlns="http://www.w3.org/2000/svg" class="dropdown-arrow" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="6 9 12 15 18 9"></polyline>
            </svg>
        </button>
        <div class="dropdown-content">
            {#each themes as theme}
                <button 
                    class="theme-option {currentTheme === theme.id ? 'active' : ''}" 
                    on:click={() => applyTheme(theme.id)}
                >
                    <span class="theme-preview {theme.id}-preview"></span>
                    <span class="theme-icon">{theme.icon}</span>
                    <span class="theme-name">{theme.name}</span>
                </button>
            {/each}
        </div>
    </div>
</div>

<style>
    .theme-selector {
        position: relative;
        z-index: 100;
    }
    
    .dropdown {
        position: relative;
        display: inline-block;
    }
    
    .theme-button {
        display: flex;
        align-items: center;
        background-color: rgba(255, 255, 255, 0.1);
        border: 1px solid rgba(255, 255, 255, 0.2);
        border-radius: 0.375rem;
        padding: 0.5rem 1rem;
        font-size: 0.875rem;
        cursor: pointer;
        transition: all 0.2s ease;
        color: inherit;
    }
    
    .theme-button:hover {
        background-color: rgba(255, 255, 255, 0.15);
    }
    
    .dropdown-arrow {
        margin-left: 0.5rem;
        transition: transform 0.2s ease;
    }
    
    .dropdown:hover .dropdown-arrow {
        transform: rotate(180deg);
    }
    
    .dropdown-content {
        position: absolute;
        right: 0;
        top: 100%;
        margin-top: 0.25rem;
        display: none;
        background-color: var(--dropdown-bg, #ffffff);
        color: var(--dropdown-text, #000000);
        min-width: 220px;
        border-radius: 0.375rem;
        box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        z-index: 1;
        overflow: hidden;
        animation: fadeIn 0.2s ease;
    }
    
    .dropdown:hover .dropdown-content {
        display: block;
    }
    
    .theme-option {
        display: flex;
        align-items: center;
        width: 100%;
        text-align: left;
        padding: 0.75rem 1rem;
        border: none;
        background: none;
        cursor: pointer;
        transition: background-color 0.15s ease;
    }
    
    .theme-option:hover {
        background-color: var(--dropdown-hover-bg, rgba(0, 0, 0, 0.05));
    }
    
    .theme-option.active {
        background-color: var(--dropdown-active-bg, rgba(0, 0, 0, 0.1));
    }
    
    .theme-icon {
        margin-right: 0.5rem;
        font-size: 1.25rem;
    }
    
    .theme-preview {
        width: 20px;
        height: 20px;
        border-radius: 50%;
        margin-right: 0.5rem;
        border: 1px solid rgba(0, 0, 0, 0.1);
        background-size: cover;
        position: relative;
        overflow: hidden;
    }
    
    /* Preview backgrounds for each theme */
    .default-preview {
        background: linear-gradient(to bottom right, #3b82f6 0%, #1d4ed8 100%);
    }
    
    .eighties-retro-preview {
        background: linear-gradient(to right, #ff00ff, #00ffff);
    }
    
    .terminal-classic-preview {
        background-color: #0a0a0a;
        position: relative;
    }
    
    .terminal-classic-preview::after {
        content: '';
        position: absolute;
        width: 50%;
        height: 50%;
        background-color: #00ff00;
        top: 25%;
        left: 25%;
    }
    
    .hand-sketched-preview {
        background-color: #f8f8f8;
        border: 2px dashed #555;
    }
    
    .steampunk-preview {
        background: radial-gradient(#d2a04a, #9e6e2c);
    }
    
    .fantasy-realm-preview {
        background: linear-gradient(to bottom, #3a0088, #4e00b9);
    }

    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-10px); }
        to { opacity: 1; transform: translateY(0); }
    }

    /* Theme specific styles for the dropdown */
    :global(.eighties-retro) .theme-button {
        border: 2px solid #ff00ff;
        background: linear-gradient(45deg, #ff00ff, #00ffff);
        color: black;
        text-shadow: 0 0 2px white;
    }

    :global(.terminal-classic) .theme-button {
        border: 1px solid #00ff00;
        background-color: black;
        color: #00ff00;
        font-family: monospace;
    }

    :global(.hand-sketched) .theme-button {
        border: 2px dashed #555;
        background-color: #f5f5f5;
    }

    :global(.steampunk) .theme-button {
        border: 2px solid #8b4513;
        background-color: #d2a04a;
        color: #5c3317;
    }

    :global(.fantasy-realm) .theme-button {
        border: 2px solid #a359ff;
        background: linear-gradient(to bottom, #3a0088, #4e00b9);
        color: #ffffff;
    }
</style>
