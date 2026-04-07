<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title id="appTitle">Reclamos CEDIS</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        :root{
            --primary:#1a56db;
            --primary-dark:#1e429f;
            --secondary:#f59e0b;
            --success:#0e9f6e;
            --danger:#e02424;
            --gray-50:#f9fafb;
            --gray-100:#f3f4f6;
            --gray-200:#e5e7eb;
            --gray-600:#4b5563;
            --gray-800:#1f2937;
        }
        body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;background:var(--gray-50);color:var(--gray-800);line-height:1.6;min-height:100vh}
        
        /* LOGIN SCREEN */
        .login-screen{min-height:100vh;display:flex;flex-direction:column;justify-content:center;align-items:center;background:linear-gradient(135deg,var(--primary) 0%,var(--primary-dark) 100%);padding:20px;position:relative}
        .login-container{background:white;padding:40px;border-radius:16px;box-shadow:0 10px 15px -3px rgba(0,0,0,0.1);width:100%;max-width:420px;position:relative;z-index:1}
        .app-icon{width:90px;height:90px;background:linear-gradient(135deg,var(--primary) 0%,var(--secondary) 100%);border-radius:20px;display:flex;align-items:center;justify-content:center;margin:0 auto 24px;font-size:40px;color:white}
        .login-container h1{text-align:center;margin-bottom:8px;font-size:28px;font-weight:700}
        .subtitle{text-align:center;color:var(--gray-600);margin-bottom:32px;font-size:14px}
        
        /* Solo visible Tienda */
        .login-simple{text-align:center;padding:20px 0}
        .login-simple h2{font-size:20px;margin-bottom:24px;color:var(--gray-800)}
        .sucursal-select{width:100%;padding:16px;border:2px solid var(--gray-200);border-radius:12px;font-size:16px;margin-bottom:20px;background:white;cursor:pointer}
        .sucursal-select:focus{outline:none;border-color:var(--primary)}
        
        .hidden-modules{display:none}
        .hidden-modules.visible{display:block;animation:fadeIn 0.3s ease}
        @keyframes fadeIn{from{opacity:0;transform:translateY(-10px)}to{opacity:1;transform:translateY(0)}}
        
        .module-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin-bottom:24px}
        .module-btn{padding:20px;border:2px solid var(--gray-200);border-radius:12px;background:white;cursor:pointer;text-align:center;transition:all 0.2s}
        .module-btn:hover{border-color:var(--primary);background:#eff6ff}
        .module-btn .icon{font-size:32px;margin-bottom:8px}
        .module-btn .label{font-weight:600;font-size:14px}
        
        .secret-hint{position:fixed;bottom:20px;right:20px;color:rgba(255,255,255,0.3);font-size:12px;cursor:help;user-select:none}
        .secret-hint:hover{color:rgba(255,255,255,0.6)}
        
        .form-group{margin-bottom:20px;text-align:left}
        .form-group label{display:block;margin-bottom:8px;font-weight:600;font-size:14px}
        .form-group input{width:100%;padding:12px 16px;border:2px solid var(--gray-200);border-radius:8px;font-size:16px}
        .btn{padding:12px 24px;border:none;border-radius:8px;font-size:16px;font-weight:600;cursor:pointer;transition:all 0.2s;width:100%}
        .btn-primary{background:var(--primary);color:white}
        .btn-primary:hover{background:var(--primary-dark)}
        .btn-secondary{background:var(--gray-100);color:var(--gray-800);border:2px solid var(--gray-200);margin-top:12px}
        
        .hidden{display:none!important}
        
        /* APP LAYOUT */
        .app-container{min-height:100vh;display:flex;flex-direction:column}
        .app-header{height:64px;background:white;border-bottom:1px solid var(--gray-200);display:flex;justify-content:space-between;align-items:center;padding:0 24px;position:sticky;top:0;z-index:100;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1)}
        .header-left{display:flex;align-items:center;gap:12px}
        .header-icon{width:40px;height:40px;background:linear-gradient(135deg,var(--primary) 0%,var(--secondary) 100%);border-radius:10px;display:flex;align-items:center;justify-content:center;color:white;font-size:20px}
        .header-title{font-size:20px;font-weight:700}
        .role-badge{padding:6px 16px;border-radius:20px;font-size:12px;font-weight:700;text-transform:uppercase}
        .role-badge.tienda{background:#dbeafe;color:#1e40af}
        
        .user-info{display:flex;align-items:center;gap:12px}
        .user-avatar{width:36px;height:36px;border-radius:50%;background:var(--primary);color:white;display:flex;align-items:center;justify-content:center;font-weight:600}
        
        .app-content{flex:1;padding:24px;max-width:1400px;margin:0 auto;width:100%}
        
        /* Stats */
        .stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:20px;margin-bottom:24px}
        .stat-card{background:white;padding:24px;border-radius:12px;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1);border-left:4px solid var(--primary)}
        .stat-card.pendiente{border-left-color:var(--secondary)}
        .stat-card.resuelto{border-left-color:var(--success)}
        .stat-value{font-size:32px;font-weight:700}
        .stat-label{font-size:14px;color:var(--gray-600);margin-top:8px}
        
        .toolbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:24px;gap:16px;flex-wrap:wrap}
        .search-box{flex:1;min-width:300px}
        .search-box input{width:100%;padding:12px 16px;border:2px solid var(--gray-200);border-radius:8px;font-size:16px}
        
        .filters{display:flex;gap:12px;margin-bottom:24px;flex-wrap:wrap}
        .filter-chip{padding:10px 20px;border-radius:20px;background:white;border:2px solid var(--gray-200);cursor:pointer;font-size:14px;font-weight:500}
        .filter-chip.active{background:var(--primary);color:white;border-color:var(--primary)}
        
        .claims-list{display:flex;flex-direction:column;gap:16px}
        .claim-card{background:white;padding:20px;border-radius:12px;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1);cursor:pointer;transition:all 0.2s;border-left:4px solid transparent}
        .claim-card:hover{transform:translateX(4px)}
        .claim-card.pendiente{border-left-color:var(--secondary)}
        .claim-card.proceso{border-left-color:var(--primary)}
        .claim-card.resuelto{border-left-color:var(--success)}
        
        .claim-header{display:flex;justify-content:space-between;margin-bottom:12px}
        .claim-id{font-weight:700;color:var(--primary);font-size:14px}
        .claim-status{padding:4px 12px;border-radius:12px;font-size:12px;font-weight:600;text-transform:uppercase}
        .claim-status.pendiente{background:#fef3c7;color:#92400e}
        .claim-status.proceso{background:#dbeafe;color:#1e40af}
        .claim-status.resuelto{background:#d1fae5;color:#065f46}
        
        .claim-title{font-size:16px;font-weight:600;margin-bottom:8px}
        .claim-meta{display:flex;gap:16px;color:var(--gray-600);font-size:13px}
        
        /* Detail */
        .detail-view{background:white;padding:32px;border-radius:16px;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1)}
        .detail-header{margin-bottom:24px;padding-bottom:24px;border-bottom:2px solid var(--gray-100)}
        .detail-title{font-size:24px;font-weight:700;margin-bottom:8px}
        
        .admin-panel{background:#f0f9ff;border:2px solid #bae6fd;border-radius:12px;padding:24px;margin-bottom:24px}
        .status-buttons{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
        .status-btn{padding:16px;border:2px solid var(--gray-200);border-radius:8px;background:white;cursor:pointer;font-weight:600;text-align:center}
        .status-btn.active.pendiente{border-color:var(--secondary);background:#fef3c7;color:#92400e}
        .status-btn.active.proceso{border-color:var(--primary);background:#dbeafe;color:#1e40af}
        .status-btn.active.resuelto{border-color:var(--success);background:#d1fae5;color:#065f46}
        
        /* Forms */
        .form-section{background:white;padding:24px;border-radius:12px;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1);margin-bottom:24px}
        .form-section h3{margin-bottom:20px;font-size:18px;font-weight:700}
        .form-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:20px}
        
        .photo-upload-area{border:3px dashed var(--gray-300);border-radius:12px;padding:48px;text-align:center;cursor:pointer;background:var(--gray-50)}
        .photo-upload-area:hover{border-color:var(--primary);background:#eff6ff}
        .photo-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:16px;margin-top:20px}
        .photo-item{position:relative;background:white;border-radius:12px;overflow:hidden;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1)}
        .photo-item img{width:100%;height:140px;object-fit:cover}
        .photo-delete{position:absolute;top:8px;right:8px;width:28px;height:28px;background:var(--danger);color:white;border:none;border-radius:50%;cursor:pointer;opacity:0;transition:opacity 0.2s}
        .photo-item:hover .photo-delete{opacity:1}
        
        .product-item{background:var(--gray-50);padding:20px;border-radius:12px;display:flex;gap:16px;margin-bottom:12px}
        .product-image{width:80px;height:80px;background:var(--gray-200);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:32px}
        
        .timeline{position:relative;padding-left:32px}
        .timeline::before{content:'';position:absolute;left:11px;top:0;bottom:0;width:2px;background:var(--gray-200)}
        .timeline-item{position:relative;margin-bottom:20px}
        .timeline-dot{position:absolute;left:-27px;width:20px;height:20px;border-radius:50%;background:var(--primary);border:4px solid white}
        
        /* Config */
        .config-nav{display:flex;gap:8px;margin-bottom:24px;flex-wrap:wrap}
        .config-nav-item{padding:12px 24px;border-radius:8px;background:white;border:2px solid var(--gray-200);cursor:pointer;font-weight:600}
        .config-nav-item.active{background:var(--primary);color:white;border-color:var(--primary)}
        .config-section{background:white;padding:32px;border-radius:12px;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1);margin-bottom:24px}
        .config-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:24px}
        .config-item{background:var(--gray-50);padding:20px;border-radius:12px}
        .config-item label{display:block;font-weight:600;margin-bottom:10px}
        .config-item input,.config-item select{width:100%;padding:12px;border:2px solid var(--gray-200);border-radius:8px}
        
        .color-picker{width:60px;height:44px;border:2px solid var(--gray-200);border-radius:8px;cursor:pointer}
        
        .user-list{display:flex;flex-direction:column;gap:12px}
        .user-item{display:flex;justify-content:space-between;align-items:center;padding:16px;background:var(--gray-50);border-radius:12px}
        
        /* Toast */
        .toast-container{position:fixed;bottom:24px;right:24px;z-index:2000;display:flex;flex-direction:column;gap:12px}
        .toast{background:var(--gray-800);color:white;padding:16px 24px;border-radius:12px;animation:slideIn 0.3s ease}
        .toast.success{background:var(--success)}
        @keyframes slideIn{from{transform:translateX(100%)}to{transform:translateX(0)}}
        
        /* Modal */
        .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,0.7);display:flex;align-items:center;justify-content:center;z-index:1000;padding:20px;opacity:0;visibility:hidden;transition:all 0.3s}
        .modal-overlay.active{opacity:1;visibility:visible}
        .modal{background:white;border-radius:16px;max-width:800px;width:100%;max-height:90vh;overflow:auto;padding:32px}
        
        @media(max-width:768px){
            .stats-grid{grid-template-columns:1fr}
            .status-buttons{grid-template-columns:1fr}
            .toolbar{flex-direction:column}
        }
    </style>
</head>
<body>

    <!-- LOGIN SCREEN - Solo Tienda visible -->
    <div id="loginScreen" class="login-screen">
        <div class="login-container">
            <div class="app-icon" id="loginIcon">📦</div>
            <h1 id="loginTitle">Reclamos CEDIS</h1>
            <p class="subtitle">Acceso para Sucursales</p>
            
            <!-- Login Simple: Solo Tienda -->
            <div id="loginSimple" class="login-simple">
                <h2>🏪 Selecciona tu Sucursal</h2>
                <select id="sucursalSelect" class="sucursal-select">
                    <option value="">-- Seleccionar Sucursal --</option>
                </select>
                <div class="form-group">
                    <label>Contraseña</label>
                    <input type="password" id="sucursalPass" placeholder="Ingresa tu contraseña">
                </div>
                <button class="btn btn-primary" onclick="loginSucursal()">Ingresar</button>
            </div>
            
            <!-- Módulos Ocultos (requieren código especial) -->
            <div id="hiddenModules" class="hidden-modules hidden">
                <div style="border-top:2px solid var(--gray-200);margin:24px 0;padding-top:24px">
                    <h3 style="margin-bottom:16px;text-align:center">🔐 Módulos Administrativos</h3>
                    <div class="module-grid">
                        <div class="module-btn" onclick="showSpecialLogin('cedis')">
                            <div class="icon">🏭</div>
                            <div class="label">CEDIS</div>
                        </div>
                        <div class="module-btn" onclick="showSpecialLogin('stats')">
                            <div class="icon">📊</div>
                            <div class="label">Estadísticas</div>
                        </div>
                        <div class="module-btn" onclick="showSpecialLogin('admin')">
                            <div class="icon">⚙️</div>
                            <div class="label">Configuración</div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Login Especial -->
            <div id="specialLogin" class="hidden">
                <button class="btn btn-secondary" onclick="backToSimple()">← Volver</button>
                <div style="margin-top:20px">
                    <div class="form-group">
                        <label>Usuario Especial</label>
                        <input type="text" id="specialUser" placeholder="Usuario">
                    </div>
                    <div class="form-group">
                        <label>Contraseña</label>
                        <input type="password" id="specialPass" placeholder="Contraseña">
                    </div>
                    <button class="btn btn-primary" onclick="loginSpecial()">Acceder</button>
                </div>
            </div>
        </div>
        
        <!-- Pista secreta (casi invisible) -->
        <div class="secret-hint" onclick="showSecretHint()" title="¿Necesitas ayuda?">© 2024</div>
    </div>

    <!-- APP CONTAINER -->
    <div id="appContainer" class="app-container hidden">
        <header class="app-header">
            <div class="header-left">
                <div class="header-icon" id="headerIcon">📦</div>
                <span class="header-title" id="headerTitle">Reclamos CEDIS</span>
                <span id="roleBadge" class="role-badge tienda">SUCURSAL</span>
            </div>
            <div class="user-info">
                <span id="displayUserName">Usuario</span>
                <div class="user-avatar" id="userAvatar">U</div>
                <button class="btn btn-secondary" onclick="logout()" style="width:auto;padding:8px 16px">Salir</button>
            </div>
        </header>

        <main class="app-content" id="mainContent">
            
            <!-- DASHBOARD -->
            <div id="viewDashboard">
                <div class="page-header" style="display:flex;justify-content:space-between;align-items:center;margin-bottom:24px">
                    <div>
                        <h1 style="font-size:28px;font-weight:700">Panel de Reclamos</h1>
                        <p style="color:var(--gray-600)">Gestiona los reclamos de tu sucursal</p>
                    </div>
                    <button class="btn btn-primary" onclick="showView('newClaim')" style="width:auto">+ Nuevo Reclamo</button>
                </div>

                <div class="stats-grid" id="quickStats"></div>

                <div class="toolbar">
                    <div class="search-box">
                        <input type="text" id="searchInput" placeholder="Buscar por ID, producto..." onkeyup="filterClaims()">
                    </div>
                </div>

                <div class="filters">
                    <div class="filter-chip active" onclick="setFilter('todos',this)">Todos</div>
                    <div class="filter-chip" onclick="setFilter('pendiente',this)">⏳ Pendientes</div>
                    <div class="filter-chip" onclick="setFilter('proceso',this)">🔄 En Proceso</div>
                    <div class="filter-chip" onclick="setFilter('resuelto',this)">✅ Resueltos</div>
                </div>

                <div class="claims-list" id="claimsList"></div>
            </div>

            <!-- NEW CLAIM -->
            <div id="viewNewClaim" class="hidden">
                <div style="margin-bottom:24px">
                    <button class="btn btn-secondary" onclick="showView('dashboard')" style="width:auto">← Volver</button>
                </div>
                
                <form onsubmit="submitClaim(event)">
                    <div class="form-section">
                        <h3>📍 Información</h3>
                        <div class="form-row">
                            <div class="form-group">
                                <label>Sucursal</label>
                                <input type="text" id="newClaimSucursal" readonly style="background:var(--gray-100)">
                            </div>
                            <div class="form-group">
                                <label>CEDIS Destino</label>
                                <input type="text" value="CEDIS Central" readonly style="background:var(--gray-100)">
                            </div>
                            <div class="form-group">
                                <label>Prioridad *</label>
                                <select required>
                                    <option value="baja">🟢 Baja</option>
                                    <option value="media" selected>🟡 Media</option>
                                    <option value="alta">🔴 Alta</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="form-section">
                        <h3>📦 Productos</h3>
                        <div id="productsContainer">
                            <div class="product-item" style="flex-wrap:wrap">
                                <div class="form-group" style="flex:1;min-width:140px">
                                    <label>SKU *</label>
                                    <input type="text" placeholder="PROD-001" required>
                                </div>
                                <div class="form-group" style="flex:2;min-width:200px">
                                    <label>Descripción *</label>
                                    <input type="text" placeholder="Nombre del producto" required>
                                </div>
                                <div class="form-group" style="width:140px">
                                    <label>Cantidad *</label>
                                    <div style="display:flex;gap:8px">
                                        <input type="number" step="0.01" min="0.01" value="1" required style="flex:1">
                                        <select style="width:60px"><option>u</option><option>kg</option></select>
                                    </div>
                                </div>
                                <div class="form-group" style="flex:2;min-width:200px">
                                    <label>Motivo *</label>
                                    <input type="text" placeholder="Ej: Producto dañado" required>
                                </div>
                            </div>
                        </div>
                        <button type="button" class="btn btn-secondary" onclick="addProduct()" style="width:auto;margin-top:12px">+ Agregar producto</button>
                    </div>

                    <div class="form-section">
                        <h3>📸 Evidencia Fotográfica</h3>
                        <div class="photo-upload-area" onclick="document.getElementById('photoInput').click()">
                            <div style="font-size:48px;margin-bottom:16px">📷</div>
                            <p style="font-weight:600;margin-bottom:8px">Haz clic para seleccionar fotos</p>
                            <p style="color:var(--gray-600);font-size:14px">Máximo 10 fotos • JPG, PNG</p>
                            <input type="file" id="photoInput" multiple accept="image/*" style="display:none" onchange="handlePhotos(event)">
                        </div>
                        <div class="photo-grid" id="photoGrid"></div>
                    </div>

                    <div class="form-section">
                        <h3>📝 Detalles</h3>
                        <div class="form-row">
                            <div class="form-group">
                                <label>Tipo de Reclamo *</label>
                                <select required>
                                    <option value="producto_danado">📦 Producto Dañado</option>
                                    <option value="faltante">📉 Faltante</option>
                                    <option value="sobrante">📈 Sobrante</option>
                                    <option value="calidad">⚠️ Problema de Calidad</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label>Acción Esperada *</label>
                                <select required>
                                    <option value="reposicion">🔄 Reposición</option>
                                    <option value="traslado">🚚 Traslado</option>
                                    <option value="301">📄 301</option>
                                </select>
                            </div>
                        </div>
                        <div class="form-group">
                            <label>Descripción detallada *</label>
                            <textarea rows="4" required></textarea>
                        </div>
                    </div>

                    <div style="display:flex;gap:16px;justify-content:flex-end">
                        <button type="button" class="btn btn-secondary" onclick="showView('dashboard')" style="width:auto">Cancelar</button>
                        <button type="submit" class="btn btn-primary" style="width:auto;min-width:200px">📤 Enviar Reclamo</button>
                    </div>
                </form>
            </div>

            <!-- DETAIL -->
            <div id="viewDetail" class="hidden">
                <div style="margin-bottom:24px">
                    <button class="btn btn-secondary" onclick="showView('dashboard')" style="width:auto">← Volver</button>
                </div>
                <div id="detailContent"></div>
            </div>

            <!-- STATS (solo admin/stats) -->
            <div id="viewStats" class="hidden">
                <h1 style="font-size:28px;font-weight:700;margin-bottom:24px">📊 Estadísticas Globales</h1>
                <div class="stats-grid" id="fullStats"></div>
                <div class="chart-container" style="background:white;padding:24px;border-radius:12px;box-shadow:0 4px 6px -1px rgba(0,0,0,0.1)">
                    <h3 style="margin-bottom:20px">Reclamos por Sucursal</h3>
                    <div id="chartSucursales"></div>
                </div>
            </div>

            <!-- CONFIG (solo admin) -->
            <div id="viewConfig" class="hidden">
                <h1 style="font-size:28px;font-weight:700;margin-bottom:24px">⚙️ Configuración</h1>
                
                <div class="config-nav">
                    <button class="config-nav-item active" onclick="showConfigTab('general',this)">General</button>
                    <button class="config-nav-item" onclick="showConfigTab('appearance',this)">Apariencia</button>
                    <button class="config-nav-item" onclick="showConfigTab('users',this)">Usuarios</button>
                </div>

                <div id="configGeneral" class="config-section">
                    <h2 style="margin-bottom:24px">Configuración General</h2>
                    <div class="config-grid">
                        <div class="config-item">
                            <label>Nombre de la App</label>
                            <input type="text" id="cfgAppName" value="Reclamos CEDIS">
                        </div>
                        <div class="config-item">
                            <label>Nombre del CEDIS</label>
                            <input type="text" id="cfgCedisName" value="CEDIS Central">
                        </div>
                    </div>
                    <button class="btn btn-success" onclick="saveConfig()" style="width:auto;margin-top:24px">💾 Guardar</button>
                </div>

                <div id="configAppearance" class="config-section hidden">
                    <h2 style="margin-bottom:24px">Personalización Visual</h2>
                    <div class="config-grid">
                        <div class="config-item">
                            <label>Color Principal</label>
                            <div style="display:flex;align-items:center;gap:12px">
                                <input type="color" class="color-picker" id="cfgPrimaryColor" value="#1a56db" onchange="previewColor(this.value)">
                                <span id="colorPreview">#1a56db</span>
                            </div>
                        </div>
                        <div class="config-item">
                            <label>Icono</label>
                            <select id="cfgIcon" onchange="previewIcon(this.value)">
                                <option value="📦">📦 Caja</option>
                                <option value="🏭">🏭 Fábrica</option>
                                <option value="🏬">🏬 Tienda</option>
                            </select>
                        </div>
                    </div>
                </div>

                <div id="configUsers" class="config-section hidden">
                    <h2 style="margin-bottom:24px">Usuarios de Sucursales (24)</h2>
                    <div class="user-list" id="userList"></div>
                </div>
            </div>

        </main>
    </div>

    <!-- PHOTO MODAL -->
    <div class="modal-overlay" id="photoModal" onclick="this.classList.remove('active')">
        <img id="previewImg" style="max-width:90%;max-height:90%;border-radius:12px">
    </div>

    <!-- TOAST -->
    <div class="toast-container" id="toastContainer"></div>

    <script>
        // ==================== CONFIGURACIÓN ====================
        // 24 Sucursales: Sucursal 01 a Sucursal 24
        const SUCURSALES = Array.from({length: 24}, (_, i) => ({
            id: `S${String(i+1).padStart(2,'0')}`,
            nombre: `Sucursal ${String(i+1).padStart(2,'0')}`,
            user: `sucursal${String(i+1).padStart(2,'0')}`,
            pass: `pass${String(i+1).padStart(2,'0')}`,
            zona: i < 8 ? 'Norte' : i < 16 ? 'Centro' : 'Sur'
        }));

        let SYSTEM_CONFIG = {
            appName: 'Reclamos CEDIS',
            cedisName: 'CEDIS Central',
            primaryColor: '#1a56db',
            appIcon: '📦',
            cedisUser: 'cedis',
            cedisPass: 'cedis2024',
            statsUser: 'admin',
            statsPass: 'stats2024',
            adminUser: 'super',
            adminPass: 'super2024'
        };

        let currentUser = null;
        let currentRole = null;
        let currentSucursal = null;
        let claimsData = [];
        let currentFilter = 'todos';
        let currentPhotos = [];
        let specialLoginType = null;

        // ==================== INICIALIZACIÓN ====================
        function init() {
            // Poblar select de sucursales
            const select = document.getElementById('sucursalSelect');
            SUCURSALES.forEach(s => {
                select.innerHTML += `<option value="${s.id}">${s.nombre} (${s.zona})</option>`;
            });
            
            generateSampleData();
            
            // Evento secreto: Triple clic en el título o Ctrl+Shift+A
            document.getElementById('loginTitle').addEventListener('click', handleSecretClick);
            document.addEventListener('keydown', handleSecretKey);
        }

        let clickCount = 0;
        let lastClickTime = 0;
        
        function handleSecretClick() {
            const now = Date.now();
            if(now - lastClickTime < 500) {
                clickCount++;
                if(clickCount >= 3) {
                    revealHiddenModules();
                    clickCount = 0;
                }
            } else {
                clickCount = 1;
            }
            lastClickTime = now;
        }

        function handleSecretKey(e) {
            // Ctrl + Shift + A = Mostrar módulos ocultos
            if(e.ctrlKey && e.shiftKey && e.key === 'A') {
                e.preventDefault();
                revealHiddenModules();
            }
        }

        function revealHiddenModules() {
            document.getElementById('hiddenModules').classList.remove('hidden');
            document.getElementById('hiddenModules').classList.add('visible');
            showToast('🔓 Módulos administrativos desbloqueados', 'success');
        }

        function showSecretHint() {
            alert('💡 Para acceder a módulos administrativos:\n\n1. Triple clic en el título "Reclamos CEDIS"\n   o\n2. Presiona Ctrl + Shift + A\n\nLuego aparecerán las opciones CEDIS, Estadísticas y Configuración.');
        }

        function generateSampleData() {
            for(let i=0; i<100; i++) {
                const suc = SUCURSALES[Math.floor(Math.random()*24)];
                claimsData.push({
                    id: `REC-2024-${String(i+1).padStart(4,'0')}`,
                    sucursalId: suc.id,
                    tienda: suc.nombre,
                    cedis: SYSTEM_CONFIG.cedisName,
                    fecha: new Date(2024,Math.floor(Math.random()*12),Math.floor(Math.random()*28)+1).toISOString(),
                    estado: ['pendiente','proceso','resuelto'][Math.floor(Math.random()*3)],
                    prioridad: ['baja','media','alta'][Math.floor(Math.random()*3)],
                    tipo: ['Producto Dañado','Faltante','Sobrante'][Math.floor(Math.random()*3)],
                    accion: ['reposicion','traslado','301'][Math.floor(Math.random()*3)],
                    productos: [{sku:`SKU-${1000+i}`,nombre:`Producto ${i+1}`,cantidad:(Math.random()*10+1).toFixed(2),unidad:'u',motivo:'Reclamo'}],
                    descripcion: 'Descripción del reclamo',
                    fotos: []
                });
            }
        }

        // ==================== LOGIN ====================
        function loginSucursal() {
            const sucId = document.getElementById('sucursalSelect').value;
            const pass = document.getElementById('sucursalPass').value;
            
            if(!sucId) { showToast('Selecciona una sucursal', 'error'); return; }
            
            const suc = SUCURSALES.find(s => s.id === sucId);
            if(pass === suc.pass) {
                currentUser = suc;
                currentRole = 'tienda';
                currentSucursal = suc.id;
                startApp();
            } else {
                showToast('Contraseña incorrecta', 'error');
            }
        }

        function showSpecialLogin(type) {
            specialLoginType = type;
            document.getElementById('loginSimple').classList.add('hidden');
            document.getElementById('hiddenModules').classList.add('hidden');
            document.getElementById('specialLogin').classList.remove('hidden');
        }

        function backToSimple() {
            document.getElementById('specialLogin').classList.add('hidden');
            document.getElementById('loginSimple').classList.remove('hidden');
            if(document.getElementById('hiddenModules').classList.contains('visible')) {
                document.getElementById('hiddenModules').classList.remove('hidden');
            }
        }

        function loginSpecial() {
            const user = document.getElementById('specialUser').value;
            const pass = document.getElementById('specialPass').value;
            
            let valid = false;
            if(specialLoginType === 'cedis' && user === SYSTEM_CONFIG.cedisUser && pass === SYSTEM_CONFIG.cedisPass) {
                currentRole = 'cedis';
                valid = true;
            } else if(specialLoginType === 'stats' && user === SYSTEM_CONFIG.statsUser && pass === SYSTEM_CONFIG.statsPass) {
                currentRole = 'stats';
                valid = true;
            } else if(specialLoginType === 'admin' && user === SYSTEM_CONFIG.adminUser && pass === SYSTEM_CONFIG.adminPass) {
                currentRole = 'admin';
                valid = true;
            }
            
            if(valid) {
                currentUser = {nombre: user.toUpperCase(), id: specialLoginType.toUpperCase()};
                currentSucursal = 'todas';
                startApp();
            } else {
                showToast('Credenciales incorrectas', 'error');
            }
        }

        function startApp() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('appContainer').classList.remove('hidden');
            
            setupUI();
            showView('dashboard');
            showToast(`Bienvenido, ${currentUser.nombre}`, 'success');
        }

        function setupUI() {
            document.getElementById('displayUserName').textContent = currentUser.nombre;
            document.getElementById('userAvatar').textContent = currentUser.nombre.substring(0,2).toUpperCase();
            
            const badge = document.getElementById('roleBadge');
            
            if(currentRole === 'tienda') {
                badge.textContent = currentUser.id;
                badge.className = 'role-badge tienda';
                // Solo dashboard y new claim visibles
            } else {
                // Roles especiales ven todo
                badge.textContent = currentRole.toUpperCase();
                badge.style.background = currentRole === 'cedis' ? '#d1fae5' : currentRole === 'stats' ? '#fef3c7' : '#fce7f3';
                badge.style.color = currentRole === 'cedis' ? '#065f46' : currentRole === 'stats' ? '#92400e' : '#9d174d';
            }
            
            // Cargar nombre de sucursal en formulario
            if(currentRole === 'tienda') {
                document.getElementById('newClaimSucursal').value = `${currentUser.id} - ${currentUser.nombre}`;
            }
        }

        // ==================== NAVEGACIÓN ====================
        function showView(view) {
            ['viewDashboard','viewNewClaim','viewDetail','viewStats','viewConfig'].forEach(v => {
                document.getElementById(v).classList.add('hidden');
            });
            document.getElementById('view'+view.charAt(0).toUpperCase()+view.slice(1)).classList.remove('hidden');
            
            if(view === 'dashboard') renderDashboard();
            else if(view === 'stats') renderStats();
            else if(view === 'config') loadConfig();
        }

        // ==================== DASHBOARD ====================
        function renderDashboard() {
            let filtered = currentRole === 'tienda' ? 
                claimsData.filter(c => c.sucursalId === currentSucursal) : 
                claimsData;
            
            const stats = {
                total: filtered.length,
                pendiente: filtered.filter(c => c.estado === 'pendiente').length,
                proceso: filtered.filter(c => c.estado === 'proceso').length,
                resuelto: filtered.filter(c => c.estado === 'resuelto').length
            };
            
            document.getElementById('quickStats').innerHTML = `
                <div class="stat-card">
                    <div class="stat-value">${stats.total}</div>
                    <div class="stat-label">Total Reclamos</div>
                </div>
                <div class="stat-card pendiente">
                    <div class="stat-value" style="color:var(--secondary)">${stats.pendiente}</div>
                    <div class="stat-label">Pendientes</div>
                </div>
                <div class="stat-card proceso">
                    <div class="stat-value" style="color:var(--primary)">${stats.proceso}</div>
                    <div class="stat-label">En Proceso</div>
                </div>
                <div class="stat-card resuelto">
                    <div class="stat-value" style="color:var(--success)">${stats.resuelto}</div>
                    <div class="stat-label">Resueltos</div>
                </div>
            `;
            
            renderClaims(filtered);
        }

        function renderClaims(data) {
            const search = document.getElementById('searchInput').value.toLowerCase();
            let filtered = data;
            
            if(currentFilter !== 'todos') {
                filtered = filtered.filter(c => c.estado === currentFilter);
            }
            
            if(search) {
                filtered = filtered.filter(c => 
                    c.id.toLowerCase().includes(search) || 
                    c.productos.some(p => p.nombre.toLowerCase().includes(search))
                );
            }
            
            filtered.sort((a,b) => new Date(b.fecha) - new Date(a.fecha));
            
            const estados = {pendiente:'Pendiente',proceso:'En Proceso',resuelto:'Resuelto'};
            
            document.getElementById('claimsList').innerHTML = filtered.map(c => `
                <div class="claim-card ${c.estado}" onclick="showDetail('${c.id}')">
                    <div class="claim-header">
                        <span class="claim-id">${c.id}</span>
                        <span class="claim-status ${c.estado}">${estados[c.estado]}</span>
                    </div>
                    <div class="claim-title">${c.tipo} • ${c.accion}</div>
                    <div class="claim-meta">
                        <span>📅 ${formatDate(c.fecha)}</span>
                        ${currentRole !== 'tienda' ? `<span>🏪 ${c.tienda}</span>` : ''}
                        <span>⚡ ${c.prioridad}</span>
                    </div>
                </div>
            `).join('');
        }

        function setFilter(status, el) {
            currentFilter = status;
            document.querySelectorAll('.filter-chip').forEach(c => c.classList.remove('active'));
            el.classList.add('active');
            renderDashboard();
        }

        function filterClaims() {
            renderDashboard();
        }

        // ==================== DETALLE ====================
        function showDetail(id) {
            const c = claimsData.find(x => x.id === id);
            if(!c) return;
            
            showView('detail');
            
            const estados = {pendiente:'Pendiente',proceso:'En Proceso',resuelto:'Resuelto'};
            const canEdit = currentRole === 'cedis' || currentRole === 'admin';
            
            const adminPanel = canEdit ? `
                <div class="admin-panel">
                    <h3>🔄 Cambiar Estatus</h3>
                    <div class="status-buttons">
                        <button class="status-btn pendiente ${c.estado==='pendiente'?'active':''}" onclick="updateStatus('${c.id}','pendiente')">⏳ Pendiente</button>
                        <button class="status-btn proceso ${c.estado==='proceso'?'active':''}" onclick="updateStatus('${c.id}','proceso')">🔄 En Proceso</button>
                        <button class="status-btn resuelto ${c.estado==='resuelto'?'active':''}" onclick="updateStatus('${c.id}','resuelto')">✅ Resuelto</button>
                    </div>
                </div>
            ` : '';
            
            document.getElementById('detailContent').innerHTML = `
                <div class="detail-view">
                    <div class="detail-header">
                        <h1 class="detail-title">${c.tipo}</h1>
                        <p style="color:var(--gray-600)">${c.id} • ${formatDateTime(c.fecha)}</p>
                    </div>
                    ${adminPanel}
                    <div class="form-section">
                        <h3>📋 Información</h3>
                        <p><strong>Sucursal:</strong> ${c.tienda}</p>
                        <p><strong>Estado:</strong> ${estados[c.estado]}</p>
                        <p><strong>Prioridad:</strong> ${c.prioridad}</p>
                    </div>
                    <div class="form-section">
                        <h3>📦 Productos</h3>
                        ${c.productos.map(p => `
                            <div class="product-item">
                                <div class="product-image">📦</div>
                                <div>
                                    <div style="font-weight:600">${p.nombre}</div>
                                    <div style="color:var(--gray-600);font-size:14px">SKU: ${p.sku} | ${p.cantidad} ${p.unidad}</div>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;
        }

        function updateStatus(id, newStatus) {
            const c = claimsData.find(x => x.id === id);
            if(!c || c.estado === newStatus) return;
            
            if(confirm(`¿Cambiar a "${newStatus.toUpperCase()}"?`)) {
                c.estado = newStatus;
                showToast('Estatus actualizado', 'success');
                showDetail(id);
            }
        }

        // ==================== NUEVO RECLAMO ====================
        function addProduct() {
            const div = document.createElement('div');
            div.className = 'product-item';
            div.style.flexWrap = 'wrap';
            div.innerHTML = `
                <div class="form-group" style="flex:1;min-width:140px"><label>SKU *</label><input type="text" required></div>
                <div class="form-group" style="flex:2;min-width:200px"><label>Descripción *</label><input type="text" required></div>
                <div class="form-group" style="width:140px">
                    <label>Cantidad *</label>
                    <div style="display:flex;gap:8px"><input type="number" step="0.01" value="1" required style="flex:1"><select style="width:60px"><option>u</option><option>kg</option></select></div>
                </div>
                <div class="form-group" style="flex:2;min-width:200px"><label>Motivo *</label><input type="text" required></div>
                <button type="button" class="btn btn-danger" onclick="this.parentElement.remove()" style="width:auto;height:40px;margin-top:20px">🗑️</button>
            `;
            document.getElementById('productsContainer').appendChild(div);
        }

        function handlePhotos(e) {
            const files = Array.from(e.target.files);
            files.forEach(file => {
                if(currentPhotos.length >= 10) return;
                const reader = new FileReader();
                reader.onload = evt => {
                    currentPhotos.push({url: evt.target.result, name: file.name});
                    updatePhotoGrid();
                };
                reader.readAsDataURL(file);
            });
        }

        function updatePhotoGrid() {
            document.getElementById('photoGrid').innerHTML = currentPhotos.map((p,i) => `
                <div class="photo-item">
                    <img src="${p.url}" onclick="document.getElementById('previewImg').src='${p.url}';document.getElementById('photoModal').classList.add('active')">
                    <button class="photo-delete" onclick="currentPhotos.splice(${i},1);updatePhotoGrid()">✕</button>
                </div>
            `).join('');
        }

        function submitClaim(e) {
            e.preventDefault();
            const nuevo = {
                id: `REC-2024-${String(claimsData.length+1).padStart(4,'0')}`,
                sucursalId: currentUser.id,
                tienda: currentUser.nombre,
                cedis: SYSTEM_CONFIG.cedisName,
                fecha: new Date().toISOString(),
                estado: 'pendiente',
                prioridad: 'media',
                tipo: 'Producto Dañado',
                accion: 'reposicion',
                productos: [{sku:'NEW',nombre:'Producto Nuevo',cantidad:'1',unidad:'u',motivo:'Nuevo'}],
                descripcion: 'Nuevo reclamo',
                fotos: currentPhotos
            };
            claimsData.unshift(nuevo);
            currentPhotos = [];
            showToast('✅ Reclamo creado: ' + nuevo.id, 'success');
            showView('dashboard');
        }

        // ==================== ESTADÍSTICAS ====================
        function renderStats() {
            const total = claimsData.length;
            const porSucursal = SUCURSALES.map(s => ({
                nombre: s.nombre,
                total: claimsData.filter(c => c.sucursalId === s.id).length
            })).sort((a,b) => b.total - a.total);
            
            const maxVal = Math.max(...porSucursal.map(s => s.total));
            
            document.getElementById('fullStats').innerHTML = `
                <div class="stat-card"><div class="stat-value">${total}</div><div class="stat-label">Total Reclamos (24 Sucursales)</div></div>
                <div class="stat-card"><div class="stat-value">${(total/24).toFixed(1)}</div><div class="stat-label">Promedio por Sucursal</div></div>
                <div class="stat-card"><div class="stat-value">${claimsData.filter(c => c.estado === 'resuelto').length}</div><div class="stat-label">Resueltos</div></div>
            `;
            
            document.getElementById('chartSucursales').innerHTML = porSucursal.slice(0, 10).map(s => `
                <div class="chart-bar" style="display:flex;align-items:center;margin-bottom:12px">
                    <div style="width:120px;font-size:14px">${s.nombre}</div>
                    <div style="flex:1;height:32px;background:var(--gray-100);border-radius:16px;overflow:hidden;margin:0 16px">
                        <div style="height:100%;width:${(s.total/maxVal)*100}%;background:var(--primary);border-radius:16px;display:flex;align-items:center;justify-content:flex-end;padding-right:12px;color:white;font-size:12px;font-weight:600">${s.total}</div>
                    </div>
                </div>
            `).join('');
        }

        // ==================== CONFIGURACIÓN ====================
        function loadConfig() {
            document.getElementById('cfgAppName').value = SYSTEM_CONFIG.appName;
            document.getElementById('cfgCedisName').value = SYSTEM_CONFIG.cedisName;
            document.getElementById('cfgPrimaryColor').value = SYSTEM_CONFIG.primaryColor;
            document.getElementById('cfgIcon').value = SYSTEM_CONFIG.appIcon;
            
            document.getElementById('userList').innerHTML = SUCURSALES.map(s => `
                <div class="user-item">
                    <div>
                        <div style="font-weight:600">${s.nombre}</div>
                        <div style="font-size:14px;color:var(--gray-600)">Usuario: ${s.user} | Pass: ${s.pass}</div>
                    </div>
                    <button class="btn btn-secondary" style="width:auto" onclick="resetSucursalPass('${s.id}')">🔄 Reset</button>
                </div>
            `).join('');
        }

        function showConfigTab(tab, btn) {
            document.querySelectorAll('.config-nav-item').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            ['configGeneral','configAppearance','configUsers'].forEach(id => {
                document.getElementById(id).classList.add('hidden');
            });
            document.getElementById('config'+tab.charAt(0).toUpperCase()+tab.slice(1)).classList.remove('hidden');
        }

        function previewColor(color) {
            document.getElementById('colorPreview').textContent = color;
            document.documentElement.style.setProperty('--primary', color);
        }

        function previewIcon(icon) {
            document.getElementById('loginIcon').textContent = icon;
            document.getElementById('headerIcon').textContent = icon;
        }

        function saveConfig() {
            SYSTEM_CONFIG.appName = document.getElementById('cfgAppName').value;
            SYSTEM_CONFIG.cedisName = document.getElementById('cfgCedisName').value;
            SYSTEM_CONFIG.primaryColor = document.getElementById('cfgPrimaryColor').value;
            SYSTEM_CONFIG.appIcon = document.getElementById('cfgIcon').value;
            
            document.getElementById('loginTitle').textContent = SYSTEM_CONFIG.appName;
            document.getElementById('headerTitle').textContent = SYSTEM_CONFIG.appName;
            
            showToast('✅ Configuración guardada', 'success');
        }

        function resetSucursalPass(id) {
            const suc = SUCURSALES.find(s => s.id === id);
            suc.pass = `pass${suc.id.substring(1)}`;
            loadConfig();
            showToast(`Contraseña reseteada: ${suc.pass}`, 'success');
        }

        // ==================== UTILIDADES ====================
        function formatDate(d) {
            return new Date(d).toLocaleDateString('es-ES', {day:'2-digit', month:'short'});
        }

        function formatDateTime(d) {
            return new Date(d).toLocaleString('es-ES', {day:'2-digit', month:'short', year:'numeric', hour:'2-digit', minute:'2-digit'});
        }

        function showToast(msg, type) {
            const t = document.createElement('div');
            t.className = `toast ${type}`;
            t.textContent = msg;
            document.getElementById('toastContainer').appendChild(t);
            setTimeout(() => t.remove(), 4000);
        }

        function logout() {
            location.reload();
        }

        // Iniciar
        init();
    </script>
</body>
</html>
