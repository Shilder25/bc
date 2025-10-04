# Guía de Despliegue CyberTrade

## Por qué no funciona en GitHub Pages

GitHub Pages solo sirve archivos estáticos (HTML, CSS, JS) y **NO puede ejecutar servidores Node.js**. Esta aplicación requiere un backend para:
- APIs de criptomonedas
- Proxy de Solana RPC
- Gestión de perfiles de wallet

## Opción 1: Desplegar en Vercel (RECOMENDADO)

### Pasos:

1. **Crear cuenta en Vercel**
   - Ve a https://vercel.com
   - Regístrate con tu cuenta de GitHub

2. **Importar el repositorio**
   - Click en "Add New" → "Project"
   - Selecciona tu repositorio de GitHub `cybertrade`
   - Vercel detectará automáticamente la configuración

3. **Configurar variables de entorno** (opcional)
   - En el dashboard de Vercel, ve a Settings → Environment Variables
   - Agrega estas variables si quieres mejorar los datos:
     ```
     CRYPTOCOMPARE_API_KEY=tu-api-key
     BIRDEYE_API_KEY=tu-api-key
     ```
   - Puedes obtener APIs gratis en:
     - CryptoCompare: https://www.cryptocompare.com/
     - Birdeye: https://docs.birdeye.so/

4. **Deploy**
   - Click en "Deploy"
   - Espera 2-3 minutos
   - Tu app estará disponible en: `https://tu-proyecto.vercel.app`

### Build Settings (ya configurado en vercel.json):
```json
{
  "version": 2,
  "builds": [
    {
      "src": "client/package.json",
      "use": "@vercel/static-build",
      "config": { "distDir": "dist" }
    },
    {
      "src": "server/index.ts",
      "use": "@vercel/node"
    }
  ]
}
```

## Opción 2: Desplegar en Render

### Pasos:

1. **Crear cuenta en Render**
   - Ve a https://render.com
   - Regístrate con tu cuenta de GitHub

2. **Crear un nuevo Web Service**
   - Click en "New" → "Web Service"
   - Conecta tu repositorio de GitHub

3. **Configuración del servicio**
   ```
   Name: cybertrade
   Region: Oregon (US West)
   Branch: main
   Root Directory: (dejar vacío)
   Runtime: Node
   Build Command: npm install && cd client && npm install && npm run build && cd .. && npm run build
   Start Command: npm start
   ```

4. **Variables de entorno** (opcional)
   - En Environment, agrega:
     ```
     NODE_ENV=production
     CRYPTOCOMPARE_API_KEY=tu-api-key
     BIRDEYE_API_KEY=tu-api-key
     ```

5. **Deploy**
   - Click en "Create Web Service"
   - Espera 3-5 minutos
   - Tu app estará disponible en: `https://cybertrade.onrender.com`

## Opción 3: Desplegar en Railway

### Pasos:

1. **Crear cuenta en Railway**
   - Ve a https://railway.app
   - Regístrate con tu cuenta de GitHub

2. **Crear nuevo proyecto**
   - Click en "New Project" → "Deploy from GitHub repo"
   - Selecciona tu repositorio

3. **Configuración automática**
   - Railway detectará Node.js automáticamente
   - Agrega variables de entorno si es necesario

4. **Deploy**
   - Railway desplegará automáticamente
   - Obtendrás una URL pública

## Verificar que el Build funciona localmente

Antes de desplegar, verifica que todo compile correctamente:

```bash
# 1. Instalar dependencias
npm install
cd client && npm install && cd ..

# 2. Build del cliente
cd client && npm run build && cd ..

# 3. Build del servidor
npm run build

# 4. Probar en producción
npm start
```

Si ves "serving on port 5000", el build fue exitoso.

## Solución de Problemas

### Error: "Cannot GET /"
- **Causa**: Estás desplegando en GitHub Pages (no soporta Node.js)
- **Solución**: Usa Vercel, Render, o Railway

### Error: "Module not found"
- **Causa**: Falta instalar dependencias
- **Solución**: Ejecuta `npm install` en raíz y en `/client`

### Error: Build failed
- **Causa**: Configuración incorrecta
- **Solución**: Verifica que `vercel.json` esté en la raíz del proyecto

## URLs útiles

- **Vercel**: https://vercel.com
- **Render**: https://render.com
- **Railway**: https://railway.app
- **CryptoCompare API**: https://www.cryptocompare.com/
- **Birdeye API**: https://docs.birdeye.so/

## Recomendación Final

**Usa Vercel** - Es la opción más simple y ya tienes la configuración lista en `vercel.json`. Solo conecta tu repo y despliega.
