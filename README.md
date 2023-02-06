# TurboRepo Study
1. [apps] repo package.json (추가 필요)

package.json
```
{
  "name": "admin",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev --port 3002",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "ui": "workspace:*", <-- 공유받을 패키지
    "@next/font": "13.1.6",
    "@types/node": "^17.0.12",
    "@types/react": "18.0.27",
    "@types/react-dom": "18.0.10",
    "eslint": "8.33.0",
    "eslint-config-next": "13.0.0",
    "next": "^13.1.1",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "typescript": "4.9.5"
  }
}
```

next.config.js [app monorepo 필수 설정]
```
module.exports = {
  reactStrictMode: true,
  transpilePackages: ["ui"],
};
```



2. 각 서비스별 구동
   1. 전체 구동
   ```
   pnpm run dev 
   ```
   
   2. 각 모노레포별 구동
   ```
   pnpm run dev --filter 이름
   ```
   
   3. tailwind 설치
      1. app monorepo에 설치
      ```
      pnpm install -D tailwindcss postcss autoprefixer --filter 이름
   
      cd apps/이름
      npx tailwindcss init
      ```

      2. 공용 컴포넌트 monorepo 설치
      ```
      pnpm install -D tailwindcss postcss autoprefixer --filter ui
   
      cd packages/ui
      npx tailwindcss init
      ```
   
      3. tailwind 설정
         1. apps monorepo tailwind.config.js
         ```
         module.exports = {
            content: [
               "./src/**/*.{js,jsx,ts,tsx}"
               ,"../../packages/ui/**/*.{js,jsx,ts,tsx}" <-- 공유받을 공유 컴포넌트 위치
            ],
            theme: {
               extend: {},
            },
            plugins: [],
         }
         ```
         
         2. ui monorepo tailwind.config.js
         ```
         module.exports = {
            content: [
                "./src/**/*.{js,jsx,ts,tsx}"
            ],
            theme: {
                extend: {},
            },
            plugins: [],
         }
         ```
         
        3. tailwind css postcss.config.js 각 디렉토리에 적용
        ```
        module.exports = {
            plugins: {
                tailwindcss: {},
                autoprefixer: {},
            },
        };
        ```


4. 빌드
```
pnpm run build --filter 이름
```