<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rolling Stock Safety: Communication & Passengers</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    
    <!-- Chosen Palette: Calm Neutrals with Slate Accent (bg-gray-50, text-gray-800, accent-slate-700) -->
    <!-- Application Structure Plan: A single-page, multi-section layout with sticky navigation. The structure is thematic, not linear like the slides, to promote exploration. It groups content into: 1) 'Overview' (Slides 1-2: Title & Pillars), 2) 'Onboard Systems' (Slides 3 & 5: An interactive tabbed view of components and requirements), 3) 'Crisis Protocol' (Slide 4: A clickable vertical stepper), and 4) 'Analysis & Recommendations' (Slides 6-10: A tabbed section with a Summary/Chart and an Accordion for Recommendations). This structure is chosen for usability, allowing users to jump to the section most relevant to them (e.g., just the recommendations) and interact with concepts (like systems or processes) in a more engaging way than static text. -->
    <!-- Visualization & Content Choices:
        - Report Info (Slide 2: Key Pillars): Goal(Inform). Viz(HTML/Tailwind cards). Interaction(Hover). Justification(Highlights core concepts). Lib(HTML/Tailwind).
        - Report Info (Slide 3, 5: Systems & Requirements): Goal(Organize/Inform). Viz(HTML/Tailwind Tabs). Interaction(Click tabs for 'P-to-C', 'C-to-P', 'Integration', 'Requirements'). Justification(Merges related concepts from two slides into one interactive component). Lib(HTML/JS).
        - Report Info (Slide 4: Crisis Protocol): Goal(Show Process). Viz(HTML/Tailwind vertical stepper). Interaction(Click phase to show details). Justification(Turns a static list into an interactive, step-by-step flow). Lib(HTML/JS).
        - Report Info (Slide 6: Summary): Goal(Inform/Compare). Viz(Radar Chart + HTML Cards). Interaction(Buttons to toggle chart data 'Current' vs. 'Recommended'). Justification(Chart visually quantifies the qualitative findings, especially the 'Human Factor' gap, making the summary more impactful). Lib(Chart.js).
        - Report Info (Slides 7-9: Recommendations): Goal(Organize). Viz(HTML/Tailwind Accordion). Interaction(Click to expand/collapse). Justification(Standard, clean UI for presenting a list of detailed items). Lib(HTML/JS).
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        :root {
            scroll-padding-top: 80px;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
        .accordion-content {
            display: none;
            overflow: hidden;
            transition: max-height 0.3s ease-out;
        }
        .accordion-button[aria-expanded="true"] + .accordion-content {
            display: block;
        }
        .accordion-button[aria-expanded="true"] .accordion-icon {
            transform: rotate(180deg);
        }
        .stepper-button[aria-selected="true"] {
            background-color: #334155;
            color: #FFFFFF;
            border-left-color: #1e293b;
        }
        .stepper-button {
            border-left-width: 4px;
        }
        .tab-button[aria-selected="true"] {
            border-bottom-color: #334155;
            color: #334155;
            font-weight: 500;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 antialiased">

    <header class="sticky top-0 z-50 bg-white/95 backdrop-blur-sm shadow-md">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            <div class="text-xl font-bold text-slate-700">
                Rolling Stock Safety
            </div>
            <div class="hidden md:flex space-x-6 text-sm font-medium">
                <a href="#overview" class="text-gray-600 hover:text-slate-700 nav-link">Overview</a>
                <a href="#systems" class="text-gray-600 hover:text-slate-700 nav-link">Onboard Systems</a>
                <a href="#protocol" class="text-gray-600 hover:text-slate-700 nav-link">Crisis Protocol</a>
                <a href="#analysis" class="text-gray-600 hover:text-slate-700 nav-link">Analysis & Recs</a>
            </div>
            <div class="md:hidden">
                <button id="mobile-menu-button" class="text-gray-600 hover:text-slate-700">
                    <span class="sr-only">Open menu</span>
                    <span class="block w-6 h-0.5 bg-current transform transition-transform"></span>
                    <span class="block w-6 h-0.5 bg-current mt-1.5 transform transition-transform"></span>
                    <span class="block w-6 h-0.5 bg-current mt-1.5 transform transition-transform"></span>
                </button>
            </div>
        </nav>
        <div id="mobile-menu" class="hidden md:hidden px-4 pt-2 pb-4 space-y-2 bg-white shadow-lg">
            <a href="#overview" class="block text-gray-600 hover:text-slate-700 nav-link">Overview</a>
            <a href="#systems" class="block text-gray-600 hover:text-slate-700 nav-link">Onboard Systems</a>
            <a href="#protocol" class="block text-gray-600 hover:text-slate-700 nav-link">Crisis Protocol</a>
            <a href="#analysis" class="block text-gray-600 hover:text-slate-700 nav-link">Analysis & Recs</a>
        </div>
    </header>

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 md:py-12">

        <section id="overview" class="mb-16">
            <h1 class="text-3xl md:text-4xl font-bold text-slate-700 text-center">Communication & Passenger Safety in Rolling Stock</h1>
            <p class="text-lg text-gray-600 text-center mt-4 max-w-3xl mx-auto">This application explores the critical link between onboard communication systems and passenger safety. Effective communication is the active layer of safety, converting hazard detection into guided, calm passenger action to prevent panic and ensure correct responses during non-routine operations.</p>
            
            <div class="grid md:grid-cols-3 gap-6 mt-12">
                <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200 hover:shadow-md transition-shadow">
                    <h3 class="text-xl font-bold text-slate-700">Timeliness</h3>
                    <p class="text-gray-600 mt-2">Immediate relay of critical information to both passengers and crew, ensuring no delay in response.</p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200 hover:shadow-md transition-shadow">
                    <h3 class="text-xl font-bold text-slate-700">Clarity</h3>
                    <p class="text-gray-600 mt-2">Unambiguous instructions (e.g., "Stay seated" vs. "Evacuate") to eliminate confusion and guide behavior.</p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200 hover:shadow-md transition-shadow">
                    <h3 class="text-xl font-bold text-slate-700">Authority</h3>
                    <p class="text-gray-600 mt-2">A calm, professional tone from crew to instill confidence and prevent the spread of panic.</p>
                </div>
            </div>
        </section>

        <section id="systems" class="mb-16">
            <h2 class="text-3xl font-bold text-slate-700 text-center">Onboard Communication Systems</h2>
            <p class="text-lg text-gray-600 text-center mt-4 max-w-3xl mx-auto">The systems on a train are a network of interconnected components. This section details the key communication pathways and the core requirements that ensure they function reliably and are accessible to all passengers. Click each tab to learn more.</p>
            
            <div class="mt-8">
                <div class="border-b border-gray-300">
                    <nav class="-mb-px flex space-x-6" aria-label="Tabs">
                        <button class="system-tab-button whitespace-nowrap py-3 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 border-transparent" data-target="systems-p2c" aria-selected="true">
                            Passenger-to-Crew
                        </button>
                        <button class="system-tab-button whitespace-nowrap py-3 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 border-transparent" data-target="systems-c2p" aria-selected="false">
                            Crew-to-Passenger
                        </button>
                        <button class="system-tab-button whitespace-nowrap py-3 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 border-transparent" data-target="systems-integration" aria-selected="false">
                            System Integration
                        </button>
                        <button class="system-tab-button whitespace-nowrap py-3 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 border-transparent" data-target="systems-requirements" aria-selected="false">
                            Core Requirements
                        </button>
                    </nav>
                </div>
                <div class="mt-6">
                    <div id="systems-p2c" class="system-tab-pane bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                        <h3 class="text-xl font-bold text-slate-700">Passenger-to-Crew (Alerting)</h3>
                        <p class="mt-2 text-gray-600">This pathway allows passengers to signal for help.</p>
                        <ul class="mt-4 space-y-2 list-disc list-inside">
                            <li><strong>Passenger Emergency Alarm (PEA):</strong> Triggers an immediate alert to the train crew or driver.</li>
                            <li><strong>Current Gap:</strong> Often a one-way, non-verbal signal (a button or pull-cord). This requires the crew to manually investigate the source, costing valuable time.</li>
                        </ul>
                    </div>
                    <div id="systems-c2p" class="system-tab-pane hidden bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                        <h3 class="text-xl font-bold text-slate-700">Crew-to-Passenger (Instruction)</h3>
                        <p class="mt-2 text-gray-600">This pathway is for providing routine information and critical emergency instructions.</p>
                        <ul class="mt-4 space-y-2 list-disc list-inside">
                            <li><strong>Public Address (PA) Systems:</strong> Essential for all audio announcements, from station stops to emergency directives.</li>
                            <li><strong>Passenger Information Systems (PIS):</strong> Digital displays that provide visual alerts, status updates, and instructions. These are critical for redundancy and accessibility.</li>
                        </ul>
                    </div>
                    <div id="systems-integration" class="system-tab-pane hidden bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                        <h3 class="text-xl font-bold text-slate-700">System Integration</h3>
                        <p class="mt-2 text-gray-600">These systems do not work in isolation. They are centrally managed to provide a unified response.</p>
                        <ul class="mt-4 space-y-2 list-disc list-inside">
                            <li><strong>Train Management System (TMS):</strong> A central computer that often manages and integrates PA, PIS, CCTV, and door controls, giving the crew a single point of control.</li>
                        </ul>
                    </div>
                    <div id="systems-requirements" class="system-tab-pane hidden bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                        <h3 class="text-xl font-bold text-slate-700">Core Requirements: Accessibility & Redundancy</h3>
                        <p class="mt-2 text-gray-600">For these systems to be effective, they must be reliable and usable by everyone.</p>
                        <ul class="mt-4 space-y-2 list-disc list-inside">
                            <li><strong>Accessibility:</strong> Visual displays (PIS) must supplement all audio announcements for the hearing impaired. Simplified, universal icons should be used.</li>
                            <li><strong>Redundancy (UPS):</strong> Systems must have uninterruptible power supplies to function during a main power loss.</li>
                            <li><strong>Redundancy (Network):</strong> Safety-critical communication networks must be separated from non-critical data (like passenger Wi-Fi).</li>
                            <li><strong>Fail-Safe:</strong> Protocols must ensure that a basic alarm signal can get through even if the complex management system fails.</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>

        <section id="protocol" class="mb-16">
            <h2 class="text-3xl font-bold text-slate-700 text-center">Communication Protocol in Crisis</h2>
            <p class="text-lg text-gray-600 text-center mt-4 max-w-3xl mx-auto">A structured communication protocol is essential to manage emergencies. It moves from initial verification to clear, actionable instructions, preventing panic and guiding passengers to safety. Click each phase to see the details.</p>

            <div class="flex flex-col md:flex-row mt-8 bg-white rounded-lg shadow-sm border border-gray-200 overflow-hidden">
                <div class="md:w-1/3 lg:w-1/4 border-b md:border-b-0 md:border-r border-gray-200">
                    <nav class="flex flex-col" aria-label="Protocol Phases">
                        <button class="stepper-button w-full text-left p-4 font-medium text-sm border-gray-200" data-target="protocol-phase1" aria-selected="true">
                            Phase 1: Incident Verification
                        </button>
                        <button class="stepper-button w-full text-left p-4 font-medium text-sm border-gray-200" data-target="protocol-phase2" aria-selected="false">
                            Phase 2: Initial Messaging
                        </button>
                        <button class="stepper-button w-full text-left p-4 font-medium text-sm border-gray-200" data-target="protocol-phase3" aria-selected="false">
                            Phase 3: Directional Messaging
                        </button>
                        <button class="stepper-button w-full text-left p-4 font-medium text-sm border-gray-200" data-target="protocol-post" aria-selected="false">
                            Post-Incident
                        </button>
                    </nav>
                </div>
                <div class="md:w-2/3 lg:w-3/4 p-6 md:p-8">
                    <div id="protocol-phase1" class="stepper-pane">
                        <h3 class="text-xl font-bold text-slate-700">Phase 1: Incident Verification</h3>
                        <p class="mt-3 text-gray-600">The moment a PEA is activated or an anomaly is detected, the crew's first priority is to verify the nature of the alarm. This is done by using CCTV and, if available, two-way voice communication with the alarm's source. This step is critical to determine if the issue is medical, a fire, a security threat, or a false alarm.</p>
                    </div>
                    <div id="protocol-phase2" class="stepper-pane hidden">
                        <h3 class="text-xl font-bold text-slate-700">Phase 2: Initial Messaging</h3>
                        <p class="mt-3 text-gray-600">Once verified, an initial message is deployed. This message is often pre-recorded or standardized to be delivered calmly and clearly. Its primary goal is to stop panic from spreading.</p>
                        <p class="mt-2 text-gray-600 italic bg-gray-100 p-3 rounded-md">Example: "We are addressing a technical issue. Please remain seated and await further instruction."</p>
                    </div>
                    <div id="protocol-phase3" class="stepper-pane hidden">
                        <h3 class="text-xl font-bold text-slate-700">Phase 3: Directional Messaging</h3>
                        <p class="mt-3 text-gray-600">If evacuation or a specific passenger action is required, messages must be clear, simple, and directional. This phase focuses on utilizing both the auditory (PA) and visual (PIS) systems simultaneously to ensure maximum reach and comprehension, especially in a noisy or chaotic environment.</p>
                    </div>
                    <div id="protocol-post" class="stepper-pane hidden">
                        <h3 class="text-xl font-bold text-slate-700">Post-Incident</h3>
                        <p class="mt-3 text-gray-600">After the immediate hazard is managed, communication shifts to next steps. This includes providing clear guidance on waiting for first responders, temporary service disruptions, or instructions for moving to a rescue train or alternative transport.</p>
                    </div>
                </div>
            </div>
        </section>
        
        <section id="analysis" class="mb-16">
            <h2 class="text-3xl font-bold text-slate-700 text-center">Analysis & Recommendations</h2>
            <p class="text-lg text-gray-600 text-center mt-4 max-w-3xl mx-auto">Based on the review of current systems and protocols, this section summarizes the key findings and provides actionable recommendations for enhancing passenger safety through improved communication.</p>

            <div class="mt-8">
                <div class="border-b border-gray-300">
                    <nav class="-mb-px flex space-x-6" aria-label="Tabs">
                        <button class="analysis-tab-button whitespace-nowrap py-3 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 border-transparent" data-target="analysis-summary" aria-selected="true">
                            Summary of Findings
                        </button>
                        <button class="analysis-tab-button whitespace-nowrap py-3 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 border-transparent" data-target="analysis-recs" aria-selected="false">
                            Recommendations
                        </button>
                    </nav>
                </div>
                <div class="mt-6">
                    <div id="analysis-summary" class="analysis-tab-pane">
                        <div class="grid md:grid-cols-2 gap-6">
                            <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                                <h3 class="text-xl font-bold text-slate-700 mb-2">Safety System Interdependence</h3>
                                <p class="text-sm text-gray-600 mb-4">This chart illustrates the gap between the current state, where technology is often adequate, and the recommended state, which requires significant improvement in human factors and protocols to be truly effective.</p>
                                <div class="chart-container">
                                    <canvas id="safetyRadarChart"></canvas>
                                </div>
                                <div class="flex justify-center space-x-4 mt-4">
                                    <button id="chart-btn-current" class="text-sm bg-slate-600 text-white py-2 px-4 rounded-md shadow-sm hover:bg-slate-700">Current State</button>
                                    <button id="chart-btn-rec" class="text-sm bg-gray-200 text-gray-700 py-2 px-4 rounded-md hover:bg-gray-300">Recommended State</button>
                                </div>
                            </div>
                            <div class="space-y-4">
                                <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                                    <h4 class="font-semibold text-slate-700">Impact</h4>
                                    <p class="text-sm text-gray-600">Effective communication is the primary tool for mitigating human error and panic during emergencies.</p>
                                </div>
                                <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                                    <h4 class="font-semibold text-slate-700">Technology Status</h4>
                                    <p class="text-sm text-gray-600">Core systems (PEA, PA) are in place, but often lack modern, two-way interaction capability.</p>
                                </div>
                                <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                                    <h4 class="font-semibold text-slate-700">Human Factor</h4>
                                    <p class="text-sm text-gray-600">The success of any system relies entirely on rigorous crew training and adherence to pre-defined crisis protocols.</p>
                                </div>
                                <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
                                    <h4 class="font-semibold text-slate-700">Need for Investment</h4>
                                    <p class="text-sm text-gray-600">Future investment must focus on standardization, accessibility, and improving the speed of information gathering by the crew.</p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div id="analysis-recs" class="analysis-tab-pane hidden">
                        <div class="space-y-4 max-w-4xl mx-auto">
                            <div class="bg-white rounded-lg shadow-sm border border-gray-200">
                                <h2>
                                    <button class="accordion-button flex justify-between items-center w-full p-5 font-medium text-left text-slate-700" aria-expanded="false" data-target="rec-1">
                                        <span>Recommendation 1: Standardized, Two-Way Interfacing</span>
                                        <span class="accordion-icon transform transition-transform duration-200">▼</span>
                                    </button>
                                </h2>
                                <div id="rec-1" class="accordion-content px-5 pb-5">
                                    <ul class="pt-2 text-gray-600 space-y-4">
                                        <li><strong>Action:</strong> Mandate the replacement of passive PEA pull-cords/buttons with <strong>two-way voice intercom units</strong>.</li>
                                        <li><strong>Benefit:</strong> Allows the crew to instantly dialogue with the passenger and verify the nature of the emergency (medical, security, fire) before applying braking or dispatching resources.</li>
                                        <li><strong>Action:</strong> Implement <strong>standardized, multilingual emergency scripts</strong> across all operating fleets to ensure message consistency and clarity.</li>
                                    </ul>
                                </div>
                            </div>
                            
                            <div class="bg-white rounded-lg shadow-sm border border-gray-200">
                                <h2>
                                    <button class="accordion-button flex justify-between items-center w-full p-5 font-medium text-left text-slate-700" aria-expanded="false" data-target="rec-2">
                                        <span>Recommendation 2: Cybersecurity & Digital Redundancy</span>
                                        <span class="accordion-icon transform transition-transform duration-200">▼</span>
                                    </button>
                                </h2>
                                <div id="rec-2" class="accordion-content px-5 pb-5">
                                    <ul class="pt-2 text-gray-600 space-y-4">
                                        <li><strong>Action:</strong> Strengthen <strong>cybersecurity measures</strong> protecting the Train Management System (TMS) and PIS network interfaces.</li>
                                        <li><strong>Benefit:</strong> Prevents malicious interference that could issue false alarms, disable PA systems, or compromise passenger confidence.</li>
                                        <li><strong>Action:</strong> Fully utilize dynamic PIS screens to display <strong>real-time safety graphics</strong> and evacuation maps, providing visual confirmation of audio messages.</li>
                                    </ul>
                                </div>
                            </div>

                            <div class="bg-white rounded-lg shadow-sm border border-gray-200">
                                <h2>
                                    <button class="accordion-button flex justify-between items-center w-full p-5 font-medium text-left text-slate-700" aria-expanded="false" data-target="rec-3">
                                        <span>Recommendation 3: Enhanced Crew Crisis Training</span>
                                        <span class="accordion-icon transform transition-transform duration-200">▼</span>
                                    </button>
                                </h2>
                                <div id="rec-3" class="accordion-content px-5 pb-5">
                                    <ul class="pt-2 text-gray-600 space-y-4">
                                        <li><strong>Action:</strong> Increase the frequency and realism of <strong>crisis simulation drills</strong> (e.g., mock medical emergencies, security incidents).</li>
                                        <li><strong>Benefit:</strong> Ensures crew members are proficient in maintaining a calm demeanor, applying correct protocol under stress, and utilizing the PA system effectively.</li>
                                        <li><strong>Action:</strong> Integrate training that focuses on communicating with diverse passengers, including those with language barriers or disabilities.</li>
                                    </ul>
                                </div>
                            </div>

                            <div class="bg-white p-6 rounded-lg shadow-sm border border-gray-200 mt-6">
                                <h3 class="text-xl font-bold text-slate-700">Conclusion & Next Steps</h3>
                                <p class="mt-3 text-gray-600">Prioritizing communication system upgrades and human protocol training is the most cost-effective way to immediately enhance rolling stock safety.</p>
                                <p class="mt-3 text-gray-600"><strong>Next Step:</strong> A full audit of current PEA/PA systems across the fleet to identify priority upgrade targets.</p>
                            </div>

                        </div>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <footer class="bg-slate-700 text-gray-300 mt-16">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 text-center text-sm">
            <p>Interactive Report generated from "Communication & Passenger Safety in Rolling Stock."</p>
            <p class="mt-1 opacity-75">This is a conceptual application and not affiliated with any railway operator.</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            
            function setupNavigation() {
                const navLinks = document.querySelectorAll('.nav-link');
                const mobileMenuButton = document.getElementById('mobile-menu-button');
                const mobileMenu = document.getElementById('mobile-menu');

                navLinks.forEach(link => {
                    link.addEventListener('click', (e) => {
                        e.preventDefault();
                        const targetId = link.getAttribute('href');
                        document.querySelector(targetId).scrollIntoView({
                            behavior: 'smooth'
                        });
                        if (mobileMenu.classList.contains('hidden') === false) {
                            mobileMenu.classList.add('hidden');
                        }
                    });
                });

                mobileMenuButton.addEventListener('click', () => {
                    mobileMenu.classList.toggle('hidden');
                    mobileMenuButton.querySelectorAll('span').forEach((span, index) => {
                        if (!mobileMenu.classList.contains('hidden')) {
                            if (index === 0) {
                                span.style.transform = 'rotate(45deg) translate(5px, 5px)';
                            } else if (index === 1) {
                                span.style.opacity = '0';
                            } else if (index === 2) {
                                span.style.transform = 'rotate(-45deg) translate(7px, -7px)';
                            }
                        } else {
                            span.style.transform = '';
                            span.style.opacity = '1';
                        }
                    });
                });
            }

            function setupTabs(buttonClass, paneClass, defaultSelected) {
                const tabButtons = document.querySelectorAll(`.${buttonClass}`);
                const tabPanes = document.querySelectorAll(`.${paneClass}`);

                tabButtons.forEach(button => {
                    button.addEventListener('click', () => {
                        const targetId = button.dataset.target;

                        tabButtons.forEach(btn => {
                            btn.setAttribute('aria-selected', 'false');
                            btn.classList.remove('tab-button-active');
                        });
                        button.setAttribute('aria-selected', 'true');
                        button.classList.add('tab-button-active');

                        tabPanes.forEach(pane => {
                            if (pane.id === targetId) {
                                pane.classList.remove('hidden');
                            } else {
                                pane.classList.add('hidden');
                            }
                        });
                    });
                });

                if (defaultSelected) {
                    document.querySelector(`.${buttonClass}[data-target="${defaultSelected}"]`).click();
                } else {
                    tabButtons[0].click();
                }
            }

            function setupStepper(buttonClass, paneClass) {
                const stepperButtons = document.querySelectorAll(`.${buttonClass}`);
                const stepperPanes = document.querySelectorAll(`.${paneClass}`);

                stepperButtons.forEach(button => {
                    button.addEventListener('click', () => {
                        const targetId = button.dataset.target;

                        stepperButtons.forEach(btn => {
                            btn.setAttribute('aria-selected', 'false');
                        });
                        button.setAttribute('aria-selected', 'true');

                        stepperPanes.forEach(pane => {
                            if (pane.id === targetId) {
                                pane.classList.remove('hidden');
                            } else {
                                pane.classList.add('hidden');
                            }
                        });
                    });
                });
                stepperButtons[0].click();
            }

            function setupAccordion() {
                document.querySelectorAll('.accordion-button').forEach(button => {
                    button.addEventListener('click', () => {
                        const isExpanded = button.getAttribute('aria-expanded') === 'true';
                        button.setAttribute('aria-expanded', !isExpanded);
                    });
                });
            }

            function setupSafetyChart() {
                const ctx = document.getElementById('safetyRadarChart').getContext('2d');

                const chartLabels = ['Technology (PEA/PA)', 'Protocols (Scripts)', 'Human Factor (Training)', 'Accessibility', 'Redundancy (UPS)'];
                
                const currentStateData = {
                    label: 'Current State',
                    data: [4, 2, 2, 3, 3],
                    fill: true,
                    backgroundColor: 'rgba(51, 65, 85, 0.2)',
                    borderColor: 'rgb(51, 65, 85)',
                    pointBackgroundColor: 'rgb(51, 65, 85)',
                    pointBorderColor: '#fff',
                    pointHoverBackgroundColor: '#fff',
                    pointHoverBorderColor: 'rgb(51, 65, 85)'
                };

                const recommendedStateData = {
                    label: 'Recommended State',
                    data: [5, 5, 5, 4, 5],
                    fill: true,
                    backgroundColor: 'rgba(25, 118, 210, 0.2)',
                    borderColor: 'rgb(25, 118, 210)',
                    pointBackgroundColor: 'rgb(25, 118, 210)',
                    pointBorderColor: '#fff',
                    pointHoverBackgroundColor: '#fff',
                    pointHoverBorderColor: 'rgb(25, 118, 210)'
                };

                const chartConfig = {
                    type: 'radar',
                    data: {
                        labels: chartLabels,
                        datasets: [currentStateData]
                    },
                    options: {
                        maintainAspectRatio: false,
                        responsive: true,
                        scales: {
                            r: {
                                beginAtZero: true,
                                max: 5,
                                min: 0,
                                ticks: {
                                    stepSize: 1
                                },
                                pointLabels: {
                                    font: {
                                        size: 10
                                    }
                                }
                            }
                        },
                        plugins: {
                            legend: {
                                position: 'top',
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        let label = context.dataset.label || '';
                                        if (label) {
                                            label += ': ';
                                        }
                                        if (context.parsed.r !== null) {
                                            label += context.parsed.r;
                                        }
                                        return label;
                                    }
                                }
                            }
                        }
                    }
                };

                const safetyRadarChart = new Chart(ctx, chartConfig);

                document.getElementById('chart-btn-current').addEventListener('click', () => {
                    safetyRadarChart.data.datasets = [currentStateData];
                    safetyRadarChart.update();
                });
                
                document.getElementById('chart-btn-rec').addEventListener('click', () => {
                    safetyRadarChart.data.datasets = [recommendedStateData];
                    safetyRadarChart.update();
                });

                document.getElementById('analysis-summary').addEventListener('click', (e) => {
                    if (e.target.id === 'chart-btn-current') {
                        e.target.classList.add('bg-slate-600', 'text-white');
                        e.target.classList.remove('bg-gray-200', 'text-gray-700');
                        document.getElementById('chart-btn-rec').classList.add('bg-gray-200', 'text-gray-700');
                        document.getElementById('chart-btn-rec').classList.remove('bg-slate-600', 'text-white');
                    } else if (e.target.id === 'chart-btn-rec') {
                        e.target.classList.add('bg-slate-600', 'text-white');
                        e.target.classList.remove('bg-gray-200', 'text-gray-700');
                        document.getElementById('chart-btn-current').classList.add('bg-gray-200', 'text-gray-700');
                        document.getElementById('chart-btn-current').classList.remove('bg-slate-600', 'text-white');
                    }
                });
                document.getElementById('chart-btn-current').click();

            }
            
            setupNavigation();
            setupTabs('system-tab-button', 'system-tab-pane', 'systems-p2c');
            setupTabs('analysis-tab-button', 'analysis-tab-pane', 'analysis-summary');
            setupStepper('stepper-button', 'stepper-pane');
            setupAccordion();
            setupSafetyChart();
        });
    </script>
</body>
</html>
