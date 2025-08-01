# Troubleshooting: npm start Issues

## Problem
`npm start` was not running and showing the error:
```
sh: react-scripts: command not found
```

## Root Causes

### 1. Invalid `react-scripts` Version
**Issue:** `package.json` had an invalid version
```json
"react-scripts": "^0.0.0"  // ❌ Invalid version
```

**Solution:** Updated to valid version
```json
"react-scripts": "5.0.1"   // ✅ Valid version
```

### 2. PostCSS Version Conflicts
**Issue:** Conflicting PostCSS versions causing dependency resolution errors
- `postcss: "^7.0.39"` (old version)
- `autoprefixer: "^10.4.21"` (requires PostCSS 8)
- `@tailwindcss/postcss7-compat` (compatibility layer)

**Error Message:**
```
npm error peer postcss@"^8.1.0" from autoprefixer@10.4.21
npm error Conflicting peer dependency: postcss@8.5.6
```

**Solution:** Updated PostCSS to v8.4.31 and removed compatibility packages

### 3. Tailwind CSS Version Issues
**Issue:** Invalid Tailwind CSS version
```json
"tailwindcss": "^4.1.11"  // ❌ Version doesn't exist (latest is 3.x)
```

**Solution:** Updated to latest stable version
```json
"tailwindcss": "^3.4.1"   // ✅ Latest stable version
```

## Steps Taken to Fix

1. **Identified the problems** by running `npm start` and `npm install`
2. **Fixed package.json** with correct versions:
   - `react-scripts: "5.0.1"`
   - `postcss: "^8.4.31"`
   - `tailwindcss: "^3.4.1"`
3. **Removed conflicting packages:**
   - `@tailwindcss/postcss`
   - `@tailwindcss/postcss7-compat`
   - `@testing-library/dom` (unnecessary)
4. **Clean install:**
   ```bash
   rm -rf node_modules package-lock.json
   npm install
   ```

## Final Working Configuration

```json
{
  "dependencies": {
    "@testing-library/jest-dom": "^6.6.4",
    "@testing-library/react": "^16.3.0",
    "@testing-library/user-event": "^13.5.0",
    "@types/jest": "^27.5.2",
    "@types/node": "^16.18.126",
    "@types/react": "^19.1.9",
    "@types/react-dom": "^19.1.7",
    "react": "^19.1.1",
    "react-dom": "^19.1.1",
    "react-scripts": "5.0.1",
    "typescript": "^4.9.5",
    "web-vitals": "^2.1.4"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.21",
    "postcss": "^8.4.31",
    "tailwindcss": "^3.4.1"
  }
}
```

## Notes

- The warnings about deprecated packages are normal for React Scripts 5.0.1
- These warnings don't affect functionality
- The development server now starts successfully with `npm start` 