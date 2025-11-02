# Dubai-gateway-contract

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UAE Opportunity Gateway - Interactive Contract</title>
    <!-- 
    A single-page application to interactively explore the UAE Opportunity Gateway service contract.
    This file translates the provided service_agreement.md into a dynamic, user-friendly dashboard.
    -->

    <!-- Chosen Palette: Navy (#0A3859), Gold (#FFC300), and Light Slate -->
    <!-- Application Structure Plan: The source report is a linear legal contract. An applicant's primary need is to find quick answers to key questions (salary, cost, risk, timeline). I have designed a tabbed, thematic dashboard structure. This is the most user-friendly way to present the information, breaking the dense text into logical, explorable chunks (Roles, Process, Guarantees) and allowing users to navigate directly to their concerns, rather than reading a long document. -->
    <!-- Visualization & Content Choices: 
        1. Report Info (Section 1: Roles/Salaries) -> Goal: Compare opportunities. -> Viz: Chart.js Bar Chart. -> Interaction: Dynamic rendering. -> Justification: A bar chart is more engaging and easier to compare salaries than an HTML table.
        2. Report Info (Section 3: Fee Structure) -> Goal: Explain the financial process and proportions. -> Viz: Chart.js Doughnut Chart + HTML "Stepper" graphic. -> Interaction: Clicking stepper buttons (Phase 1, 2, 3) could highlight parts of the chart (though here it's presented as a clear visual flow). -> Justification: A doughnut chart instantly shows the 10%/50%/40% split, building trust by visualizing the small initial commitment. The stepper clarifies the timeline.
        3. Report Info (Section 4: Guarantees/Timelines) -> Goal: Inform about risk and safety nets. -> Viz: HTML/Tailwind "Callout Cards" (no chart). -> Interaction: Static, but high-visibility. -> Justification: This critical policy info (refunds, deadlines) needs to be unmissable, and bold typography with strong-colored borders (green for guarantee, red for deadline) is the most effective method.
        4. Report Info (Section 2: Application) -> Goal: Convert user. -> Viz: Large, clear HTML/Tailwind button. -> Interaction: Link to Jotform. -> Justification: The primary CTA must be clear and persistent.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@3.7.1/dist/chart.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">

    <style>
        :root {
            --color-navy: #0A3859;
            --color-gold: #FFC300;
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* light slate background */
        }
        .text-navy { color: var(--color-navy); }
        .bg-navy { background-color: var(--color-navy); }
        .text-gold { color: var(--color-gold); }
        .bg-gold { background-color: var(--color-gold); }
        .border-gold { border-color: var(--color-gold); }

        .nav-link {
            @apply px-4 py-3 text-sm font-semibold text-gray-600 border-b-2 border-transparent cursor-pointer transition-all duration-200;
        }
        .nav-link.active, .nav-link:hover {
            @apply text-navy border-navy;
        }
        .content-section {
            @apply hidden;
        }
        .content-section.active {
            @apply block;
        }
        
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
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
    </style>
</head>
<body class="text-gray-800">

    <div class="min-h-screen">
        <!-- Header -->
        <header class="bg-white shadow-sm">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-5 flex flex-col md:flex-row justify-between items-center">
                <div>
                    <h1 class="text-3xl font-extrabold text-navy">UAE Opportunity Gateway</h1>
                    <p class="text-lg text-gray-600">Your Interactive Service Contract</p>
                </div>
                <a href="https://form.jotform.com/252864660359467" target="_blank" rel="noopener noreferrer"
                   class="bg-gold text-navy font-bold py-3 px-6 rounded-lg shadow-md hover:bg-opacity-90 transition-transform duration-200 hover:scale-105 mt-4 md:mt-0">
                    Apply Now
                </a>
            </div>
        </header>

        <!-- Navigation -->
        <nav class="bg-white border-b border-gray-200">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-center -mb-px">
                    <a class="nav-link active" data-target="roles">Available Roles & Salaries</a>
                    <a class="nav-link" data-target="process">The 3-Phase Process</a>
                    <a class="nav-link" data-target="guarantees">Guarantees & Timelines</a>
                    <a class="nav-link" data-target="contact">Contact & Apply</a>
                </div>
            </div>
        </nav>

        <!-- Main Content -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 md:py-12">

            <!-- Section 1: Available Roles & Salaries -->
            <section id="roles" class="content-section active space-y-8">
                <div class="text-center">
                    <h2 class="text-3xl font-bold text-navy mb-2">Available Roles & Salaries</h2>
                    <p class="text-lg text-gray-600 max-w-3xl mx-auto">Explore our current priority roles. All positions include a full employer-paid package: <span class="font-semibold">Employment Visa, Housing, and Food.</span></p>
                </div>
                
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-gray-100">
                    <h3 class="text-xl font-bold text-navy mb-6 text-center">Guaranteed Minimum Monthly Salary (AED)</h3>
                    <div class="chart-container">
                        <canvas id="rolesChart"></canvas>
                    </div>
                    <p class="text-center text-sm text-gray-500 mt-4">Salary shown in AED (United Arab Emirates Dirham). All roles include a full package.</p>
                </div>
            </section>

            <!-- Section 2: The 3-Phase Process -->
            <section id="process" class="content-section space-y-8">
                <div class="text-center">
                    <h2 class="text-3xl font-bold text-navy mb-2">The 3-Phase Financial Process</h2>
                    <p class="text-lg text-gray-600 max-w-3xl mx-auto">Our service is premium and transparent. There is **NO FEE** to apply or interview. A phased service fee is required only <span class="font-semibold">after you pass the video interview</span> and sign a formal contract.</p>
                </div>

                <!-- Stepper Graphic -->
                <div class="w-full max-w-5xl mx-auto px-4 py-6">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <!-- Step 1 -->
                        <div class="flex flex-col items-center text-center p-4">
                            <div class="flex items-center justify-center bg-gold text-navy w-12 h-12 rounded-full font-extrabold text-2xl shadow-md">1</div>
                            <h4 class="font-bold text-lg text-navy mt-4 mb-1">Phase 1: Contract</h4>
                            <p class="text-gray-600 text-sm">Due **24 hours** after passing your interview. This initiates your legal file.</p>
                        </div>
                        <!-- Step 2 -->
                        <div class="flex flex-col items-center text-center p-4">
                            <div class="flex items-center justify-center bg-gold text-navy w-12 h-12 rounded-full font-extrabold text-2xl shadow-md">2</div>
                            <h4 class="font-bold text-lg text-navy mt-4 mb-1">Phase 2: Visa</h4>
                            <p class="text-gray-600 text-sm">Due upon **successful issuance** of your employment visa. This covers final logistics.</p>
                        </div>
                        <!-- Step 3 -->
                        <div class="flex flex-col items-center text-center p-4">
                            <div class="flex items-center justify-center bg-gold text-navy w-12 h-12 rounded-full font-extrabold text-2xl shadow-md">3</div>
                            <h4 class="font-bold text-lg text-navy mt-4 mb-1">Phase 3: Arrival</h4>
                            <p class="text-gray-600 text-sm">Due **30 days** after your arrival and starting work in Dubai.</p>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
                    <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-gray-100">
                        <h3 class="text-xl font-bold text-navy mb-6 text-center">Fee Proportion (Total: 9,442.50 AED)</h3>
                        <div class="chart-container" style="max-height: 350px;">
                            <canvas id="feesChart"></canvas>
                        </div>
                    </div>
                    <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-gray-100">
                        <h3 class="text-xl font-bold text-navy mb-6 text-center">Fee Breakdown by Phase</h3>
                        <ul class="space-y-4">
                            <li class="flex justify-between items-center p-4 bg-slate-50 rounded-lg">
                                <div>
                                    <span class="font-semibold text-navy">Phase 1: Contract Acceptance</span>
                                    <span class="block text-sm text-gray-500">10% of Total</span>
                                </div>
                                <span class="font-bold text-lg text-navy">944.25 AED</span>
                            </li>
                            <li class="flex justify-between items-center p-4 bg-slate-50 rounded-lg">
                                <div>
                                    <span class="font-semibold text-navy">Phase 2: Visa Issuance</span>
                                    <span class="block text-sm text-gray-500">50% of Total</span>
                                </div>
                                <span class="font-bold text-lg text-navy">4,721.25 AED</span>
                            </li>
                            <li class="flex justify-between items-center p-4 bg-slate-50 rounded-lg">
                                <div>
                                    <span class="font-semibold text-navy">Phase 3: Post-Arrival</span>
                                    <span class="block text-sm text-gray-500">40% of Total</span>
                                </div>
                                <span class="font-bold text-lg text-navy">3,777.00 AED</span>
                            </li>
                        </ul>
                    </div>
                </div>
            </section>

            <!-- Section 3: Guarantees & Timelines -->
            <section id="guarantees" class="content-section space-y-8">
                <div class="text-center">
                    <h2 class="text-3xl font-bold text-navy mb-2">Your Guarantees & Commitments</h2>
                    <p class="text-lg text-gray-600 max-w-3xl mx-auto">We operate with 100% transparency. Here are our core policies on refunds and required timelines.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <!-- Guarantee Card -->
                    <div class="bg-white p-8 rounded-xl shadow-lg border-l-8 border-green-500">
                        <div class="flex items-center mb-4">
                            <span class="flex items-center justify-center w-12 h-12 rounded-full bg-green-100">
                                <span class="text-2xl">🛡️</span>
                            </span>
                            <h3 class="text-2xl font-bold text-green-700 ml-4">100% Refund Guarantee</h3>
                        </div>
                        <p class="text-gray-700 text-lg">
                            The **Phase 1 Fee (944.25 AED)** is <span class="font-extrabold">FULLY REFUNDABLE</span> if your employment visa is officially rejected by UAE authorities for reasons outside of your control.
                        </p>
                        <p class="text-gray-600 mt-3 text-sm">This is our commitment to a secure, risk-managed process for all serious candidates.</p>
                    </div>

                    <!-- Deadline Card -->
                    <div class="bg-white p-8 rounded-xl shadow-lg border-l-8 border-yellow-500">
                        <div class="flex items-center mb-4">
                            <span class="flex items-center justify-center w-12 h-12 rounded-full bg-yellow-100">
                                <span class="text-2xl">✈️</span>
                            </span>
                            <h3 class="text-2xl font-bold text-yellow-700 ml-4">Strict 2-Week Deadline</h3>
                        </div>
                        <p class="text-gray-700 text-lg">
                            Applicants <span class="font-extrabold">MUST</span> be ready to mobilize. You have a strict period of **Two (2) Weeks** from the date your visa is issued to book and confirm your flight to Dubai.
                        </p>
                        <p class="text-gray-600 mt-3 text-sm">Failure to meet this deadline will result in contract termination and forfeiture of fees paid.</p>
                    </div>
                </div>
            </section>

            <!-- Section 4: Contact & Apply -->
            <section id="contact" class="content-section space-y-8">
                <div class="text-center bg-navy text-white p-10 md:p-16 rounded-xl shadow-2xl">
                    <h2 class="text-4xl font-extrabold mb-4">Start Your Dubai Career Today</h2>
                    <p class="text-xl text-gray-300 max-w-2xl mx-auto mb-8">
                        You have reviewed the roles, the process, and the guarantees. If you are a serious candidate ready for a high-value, high-commitment opportunity, we invite you to apply.
                    </p>
                    <a href="https://form.jotform.com/252864660359467" target="_blank" rel="noopener noreferrer"
                       class="bg-gold text-navy text-xl font-bold py-4 px-10 rounded-lg shadow-md hover:bg-opacity-90 transition-transform duration-200 hover:scale-105">
                        Start Your Official Application
                    </a>
                    <div class="mt-10 border-t border-gray-700 pt-6">
                        <h4 class="text-lg font-semibold text-gray-200">Contact Information</h4>
                        <p class="text-gray-300 mt-2">Chris Luis | UAE Gateway Advisor</p>
                        <p class="text-gold text-lg font-semibold mt-1">WhatsApp: +971 52 136 0180</p>
                        <p class="text-gray-300 text-sm mt-1">Email: uaeopportunitygateway@gmail.com</p>
                        <p class="text-gray-400 text-xs mt-2">Office Location: Dubai, Abu Hail, United Arab Emirates</p>
                    </div>
                </div>
            </section>

        </main>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const navLinks = document.querySelectorAll('.nav-link');
            const contentSections = document.querySelectorAll('.content-section');

            // Navigation logic
            navLinks.forEach(link => {
                link.addEventListener('click', () => {
                    const targetId = link.getAttribute('data-target');

                    navLinks.forEach(nav => nav.classList.remove('active'));
                    link.classList.add('active');

                    contentSections.forEach(section => {
                        if (section.id === targetId) {
                            section.classList.add('active');
                        } else {
                            section.classList.remove('active');
                        }
                    });
                });
            });

            // Chart Data
            const rolesData = {
                labels: ['Delivery Agent', 'Technicians (Skilled)', 'Security Guard'],
                salaries: [4000, 2200, 1800],
                fcfa: ['~600,000+ FCFA', '~330,000+ FCFA', '~270,000+ FCFA']
            };

            const feesData = {
                labels: ['Phase 1: Contract (10%)', 'Phase 2: Visa (50%)', 'Phase 3: Arrival (40%)'],
                values: [944.25, 4721.25, 3777.00],
            };
            
            // Helper function to wrap labels (as per instructions)
            function wrapLabel(str, maxWidth) {
                if (str.length <= maxWidth) return str;
                let wrapped = '';
                let currentLine = '';
                str.split(' ').forEach(word => {
                    if ((currentLine + word).length > maxWidth) {
                        wrapped += currentLine + '\n';
                        currentLine = '';
                    }
                    currentLine += word + ' ';
                });
                return wrapped + currentLine.trim();
            }

            // Create Roles Chart (Bar)
            const rolesCtx = document.getElementById('rolesChart');
            if (rolesCtx) {
                new Chart(rolesCtx, {
                    type: 'bar',
                    data: {
                        labels: rolesData.labels,
                        datasets: [{
                            label: 'Minimum Monthly Salary (AED)',
                            data: rolesData.salaries,
                            backgroundColor: 'rgba(10, 56, 89, 0.8)', // Navy
                            borderColor: 'rgba(10, 56, 89, 1)',
                            borderWidth: 1,
                            borderRadius: 4
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                display: false
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        let label = context.dataset.label || '';
                                        if (label) {
                                            label += ': ';
                                        }
                                        if (context.parsed.y !== null) {
                                            label += `${context.parsed.y} AED`;
                                        }
                                        return label;
                                    },
                                    afterLabel: function(context) {
                                        return `(Approx. ${rolesData.fcfa[context.dataIndex]})`;
                                    }
                                }
                            }
                        },
                        scales: {
                            y: {
                                beginAtZero: true,
                                title: {
                                    display: true,
                                    text: 'Salary in AED'
                                }
                            },
                            x: {
                                ticks: {
                                     callback: function(value) {
                                        return wrapLabel(this.getLabelForValue(value), 10);
                                    }
                                }
                            }
                        }
                    }
                });
            }

            // Create Fees Chart (Doughnut)
            const feesCtx = document.getElementById('feesChart');
            if (feesCtx) {
                new Chart(feesCtx, {
                    type: 'doughnut',
                    data: {
                        labels: feesData.labels,
                        datasets: [{
                            label: 'Fee Portion',
                            data: feesData.values,
                            backgroundColor: [
                                'rgba(255, 195, 0, 0.7)', // Gold
                                'rgba(10, 56, 89, 0.7)',  // Navy
                                'rgba(10, 56, 89, 0.5)'   // Lighter Navy
                            ],
                            borderColor: [
                                'rgba(255, 195, 0, 1)',
                                'rgba(10, 56, 89, 1)',
                                'rgba(10, 56, 89, 1)'
                            ],
                            borderWidth: 2
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                position: 'bottom',
                                labels: {
                                    font: {
                                        size: 11
                                    }
                                }
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        let label = context.label || '';
                                        if (label) {
                                            label += ': ';
                                        }
                                        if (context.parsed !== null) {
                                            label += `${context.parsed} AED`;
                                        }
                                        return label;
                                    }
                                }
                            }
                        }
                    }
                });
            }
        });
    </script>

</body>
</html>
