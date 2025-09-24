<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Site-to-Site VPN Connection Setup on Azure</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f9f9f9;
      margin: 0;
      padding: 0;
      line-height: 1.6;
      color: #333;
    }
    header {
      background: #0078D7;
      color: white;
      padding: 20px;
      text-align: center;
    }
    header h1 {
      margin: 0;
      font-size: 2rem;
    }
    section {
      max-width: 900px;
      margin: auto;
      padding: 20px;
      background: white;
      border-radius: 10px;
      box-shadow: 0 0 8px rgba(0,0,0,0.1);
      margin-bottom: 20px;
    }
    h2 {
      color: #0078D7;
      margin-top: 30px;
    }
    h3 {
      margin-top: 20px;
      color: #444;
    }
    p {
      margin: 10px 0;
    }
    ul {
      margin: 10px 0 20px 20px;
    }
    .step {
      background: #f1f8ff;
      border-left: 5px solid #0078D7;
      padding: 10px 15px;
      margin-bottom: 15px;
      border-radius: 5px;
    }
    img {
      max-width: 100%;
      border: 1px solid #ddd;
      border-radius: 6px;
      margin: 15px 0;
    }
    a {
      color: #0078D7;
      text-decoration: none;
    }
    a:hover {
      text-decoration: underline;
    }
    footer {
      text-align: center;
      padding: 20px;
      font-size: 0.9rem;
      background: #f1f1f1;
    }
  </style>
</head>
<body>

<header>
  <h1>Site-to-Site VPN Connection Setup on Azure</h1>
  <p>Step-by-step lab project to configure secure VPN connectivity between on-prem and Azure</p>
</header>

<section>
  <h2>📌 Overview</h2>
  <p>This project demonstrates how to set up a <strong>Site-to-Site (S2S) VPN</strong> between an on-premises environment and Azure.</p>
  <p>A Site-to-Site VPN is an IPsec VPN tunnel that securely connects two networks over the public internet.</p>
  <ul>
    <li><strong>Example:</strong> Connect your on-premises datacenter/office network to an Azure Virtual Network (VNet).</li>
    <li>After setup, resources on both sides can communicate as if they were on the same local network.</li>
  </ul>
</section>

<section>
  <h2>🔑 What is S2S VPN?</h2>
  <p>A Site-to-Site VPN connects entire networks together using encrypted IPsec/IKE tunnels.</p>
  <ul>
    <li><strong>On-Prem Network:</strong> 192.168.1.0/24</li>
    <li><strong>Azure VNet:</strong> 10.0.0.0/16</li>
    <li><strong>Example Communication:</strong> 
      <ul>
        <li>Azure VM → On-prem File Server (192.168.1.10)</li>
        <li>On-prem Server → Azure VM (10.0.0.4)</li>
      </ul>
    </li>
  </ul>
  <p>All traffic between the two networks is <strong>encrypted</strong>.</p>
</section>

<section>
  <h2>🏗️ Lab Environment</h2>
  <ul>
    <li><strong>On-Prem VNet</strong>: 172.0.0.0/16 (Gateway Subnet: 172.0.1.0/27)</li>
    <li><strong>Hub VNet</strong>: 10.100.0.0/16 (Gateway Subnet: 10.100.1.0/27)</li>
    <li><strong>Spoke VNet</strong>: 10.200.0.0/16 (Central India)</li>
    <li><strong>Gateways:</strong> VNet Gateway on OnPrem + Hub, Local Network Gateways for routing</li>
    <li><strong>VPN Connections:</strong> Bidirectional connections between onpremvnet ↔ hubvnet</li>
  </ul>
</section>

<section>
  <h2>⚙️ Setup Steps</h2>
  
  <div class="step">
    <h3>Step 1: Create Resource Groups</h3>
    <p>Create separate resource groups for hub, spoke, and on-prem VNets.</p>
    <img src="image.png" alt="Resource group creation">
  </div>
  
  <div class="step">
    <h3>Step 2: Create VNets</h3>
    <p>Create 3 VNets (hub, spoke, on-prem) within the respective resource groups.</p>
    <img src="image-1.png" alt="VNet creation">
    <img src="image-2.png" alt="VNet setup">
  </div>
  
  <div class="step">
    <h3>Step 3: Create Virtual Network Gateways</h3>
    <p>Deploy Virtual Network Gateway and Local Network Gateway on both On-Prem and Hub VNets.</p>
    <img src="image-3.png" alt="VNet Gateway setup">
  </div>
  
  <div class="step">
    <h3>Step 4: Create Local Network Gateways</h3>
    <p>Enter the on-premises public IP and Azure hub address ranges. Repeat for both Hub & On-Prem.</p>
    <img src="image-4.png" alt="Local Network Gateway">
    <img src="image-5.png" alt="Local Network Gateway config">
  </div>
  
  <div class="step">
    <h3>Step 5: Create VPN Connections</h3>
    <p>Establish bidirectional connections between on-prem and hub using shared keys.</p>
    <img src="image-6.png" alt="VPN connection 1">
    <img src="image-7.png" alt="VPN connection 2">
    <img src="image-8.png" alt="Connection established">
  </div>

</section>

<section>
  <h2>✅ Validation</h2>
  <p>Deploy a jump host in the spoke VNet, test connectivity, and configure NSG + Windows Firewall rules for ICMP/RDP.</p>
  <img src="image-11.png" alt="NSG rule setup">
  <img src="image-12.png" alt="Windows Firewall ICMP">
  <img src="image-14.png" alt="TNC test">
</section>

<section>
  <h2>📖 References</h2>
  <ul>
    <li><a href="https://learn.microsoft.com/en-us/azure/vpn-gateway/tutorial-site-to-site-portal">Azure S2S VPN Tutorial</a></li>
    <li><a href="https://learn.microsoft.com/en-us/azure/virtual-network-manager/tutorial-create-secured-hub-and-spoke">Hub-Spoke Topology Guide</a></li>
    <li><a href="https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-peering-gateway-transit">Gateway Transit in Hub-Spoke</a></li>
  </ul>
</section>

<footer>
  <p>💡 Written for learning & practice. Feedback welcome! 🚀</p>
</footer>

</body>
</html>
