# Local9Pay 🎸

Gestión de pagos para local de ensayo. Construido con HTML vanilla + Supabase.

## Stack
- **Frontend**: HTML + CSS + JS (un solo archivo)
- **Backend / DB**: Supabase (Auth + PostgreSQL)
- **Hosting**: GitHub Pages o cualquier hosting estático

---

## 1. Configurar Supabase

### 1.1 Ejecutar el SQL
Ve a tu proyecto Supabase → **SQL Editor** y pega el contenido de `supabase_setup.sql`. Ejecuta.

Esto crea:
- Tabla `profiles` (usuarios + rol admin/user)
- Tabla `config` (configuración del local)
- Tabla `payments` (pagos mensuales)
- Row Level Security (RLS) para que cada usuario solo vea sus datos
- Trigger que crea perfil automáticamente al registrar usuario

### 1.2 Crear usuario admin
En Supabase → **Authentication → Users → Invite user**:
- Email: el tuyo
- Luego en **SQL Editor** ejecuta:
```sql
UPDATE public.profiles SET role = 'admin' WHERE id = 'TU-UUID-AQUI';
```

### 1.3 Deshabilitar confirmación de email (opcional para pruebas)
Supabase → **Authentication → Providers → Email** → desactiva "Confirm email"

---

## 2. Subir a GitHub

```bash
git clone https://github.com/skudeiro-svg/local9pay.git
cd local9pay
# Copia index.html, supabase_setup.sql y README.md aquí
git add .
git commit -m "Initial Local9Pay setup"
git push origin main
```

---

## 3. Publicar con GitHub Pages

GitHub → tu repo → **Settings → Pages → Source: main branch / root**

URL resultante: `https://skudeiro-svg.github.io/local9pay`

---

## 4. Emails reales (opcional)

Para envío real del recordatorio del día 28, crea una **Supabase Edge Function**:

```bash
supabase functions new send-reminder
```

Usa [Resend](https://resend.com) (gratis hasta 100 emails/día):
```typescript
import { Resend } from 'npm:resend';
const resend = new Resend(Deno.env.get('RESEND_KEY'));
await resend.emails.send({
  from: 'sala@tudominio.com',
  to: user.email,
  subject: '📅 Cuota de ensayo',
  html: `...`
});
```

Activa el cron en `supabase/functions/send-reminder/index.ts` para que se ejecute el día 28 de cada mes.

---

## Credenciales de ejemplo para pruebas

Crea estos usuarios desde el panel de Usuarios de la app (con sesión admin):
- Cualquier email + contraseña mín. 6 caracteres

