# Publishing Checklist - @deanfinancials/calculators

**Quick reference for publishing new versions**

---

## Pre-Publish Checklist

- [ ] All changes committed to git
- [ ] Code reviewed and tested locally
- [ ] README updated with new features (if applicable)
- [ ] CHANGELOG_AND_IMPLEMENTATION_LOG.md updated
- [ ] All imports use `.js` extensions in `src/index.ts`
- [ ] `moduleResolution: "node"` in tsconfig.json
- [ ] No runtime dependencies added

---

## Build & Verify

```bash
# Build package
npm run build

# Verify .js extensions in output
cat dist/index.js | grep "from './"
# Should see: export * from './retirement/retirement.js';
```

- [ ] Build completed without errors
- [ ] All imports have `.js` extensions in dist/index.js

---

## Version Update

```bash
# Choose appropriate version bump:
npm version patch   # Bug fixes (1.0.0 → 1.0.1)
npm version minor   # New features (1.0.0 → 1.1.0)
npm version major   # Breaking changes (1.0.0 → 2.0.0)
```

- [ ] Version updated in package.json
- [ ] Version number follows semver
- [ ] Git tag created automatically

---

## Publish to npm

```bash
# Optional: Dry run first
npm publish --dry-run

# Publish to npm
npm publish --access public
```

- [ ] Published successfully
- [ ] No errors in output
- [ ] Version visible on https://www.npmjs.com/package/@deanfinancials/calculators

---

## **CRITICAL: Update Consumer Projects**

### deanfi-website

```bash
cd /path/to/deanfi-website

# Update to latest version
npm install @deanfinancials/calculators@latest

# Verify update
npm list @deanfinancials/calculators
# Should show new version (e.g., @deanfinancials/calculators@1.0.1)

# Check diff
git diff package.json package-lock.json
```

- [ ] package.json shows new version
- [ ] package-lock.json updated
- [ ] Both files staged for commit

### Test Calculator Functionality

```bash
npm run dev
```

Visit these calculator pages and test:
- [ ] http://localhost:4321/debt/debt-payoff
  - [ ] Add multiple debts
  - [ ] Adjust extra payment slider
  - [ ] Click "Calculate Payoff Plan"
  - [ ] Verify results display
  - [ ] Check chart renders
  - [ ] Test strategy switching
  - [ ] Verify calculations correct

- [ ] Other calculator pages (when implemented)

### Build Verification

```bash
npm run build
```

- [ ] Build completes without errors
- [ ] No TypeScript errors
- [ ] No import errors

### Commit & Deploy

```bash
git add package.json package-lock.json
git commit -m "chore: update @deanfinancials/calculators to vX.X.X"
git push
```

- [ ] Changes committed
- [ ] Pushed to GitHub
- [ ] Cloudflare Pages deployment triggered
- [ ] Production deployment successful

---

## Post-Publish Verification

### npm Registry

- [ ] Visit https://www.npmjs.com/package/@deanfinancials/calculators
- [ ] New version shows in version history
- [ ] Package size reasonable (~25 kB)
- [ ] README displays correctly

### Consumer Installation Test

```bash
# In a fresh directory
mkdir test-install && cd test-install
npm init -y
npm install @deanfinancials/calculators

# Test import
node -e "import('@deanfinancials/calculators').then(c => console.log(Object.keys(c)))"
```

- [ ] Package installs without errors
- [ ] Imports work correctly
- [ ] All exports available

### Production Verification

- [ ] Visit https://deanfi.com/debt/debt-payoff
- [ ] Calculator loads without errors
- [ ] Calculations produce correct results
- [ ] No console errors in browser DevTools

---

## If Something Goes Wrong

### Published wrong version

```bash
# Deprecate the bad version
npm deprecate @deanfinancials/calculators@1.0.X "Broken build, use 1.0.Y instead"

# Publish fixed version
npm version patch
npm publish --access public
```

### Consumer not getting updates

```bash
# In consumer project
npm cache clean --force
rm -rf node_modules package-lock.json
npm install @deanfinancials/calculators@latest
```

### Module not found errors

Check:
- [ ] All imports in `src/index.ts` have `.js` extensions
- [ ] `moduleResolution: "node"` in tsconfig.json
- [ ] Rebuild and republish with patch version

---

## Quick Command Reference

```bash
# Build
npm run build

# Version bump
npm version patch|minor|major

# Publish
npm publish --access public

# Update consumer (in deanfi-website)
npm install @deanfinancials/calculators@latest

# Test consumer
npm run dev

# Build consumer
npm run build

# Verify version
npm list @deanfinancials/calculators
```

---

## Remember

✅ **ALWAYS** update consumer projects after publishing  
✅ **ALWAYS** test calculator pages after updating  
✅ **ALWAYS** commit package.json AND package-lock.json  
✅ **ALWAYS** verify production deployment

❌ **NEVER** skip consumer updates  
❌ **NEVER** commit with file:// paths in package.json  
❌ **NEVER** publish without testing locally first

---

**Last Updated:** November 21, 2025  
**Next Review:** After first few publishes, update this checklist based on experience
