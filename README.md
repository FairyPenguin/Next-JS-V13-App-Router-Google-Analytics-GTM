*Notice: in case of any errors regarding NextJS
This is a known bug and can be resolved by wrapping your Analytics components in the root layout with a Suspense.  Next.js bug: https://github.com/vercel/next.js/issues/48442#issuecomment-1519139562*



## Next-JS-V13-App-Router-Google-Analytics-GTM

Next JS V13+ App Router Google Analytics&amp;GTM Integration 

### 1-Create a new component **GoogleAnalytics.jsx**


```js
"use client"

import Script from "next/script"
import { usePathname, useSearchParams } from "next/navigation"
import { pageView } from "@/lib/gtagHelper"
import { useEffect } from "react"


function GoogleAnalytics({ GA_MEASUREMENT_ID }) {

    const pathname = usePathname()
    const searchParams = useSearchParams()

    useEffect(() => {
        const url = pathname + searchParams.toString()

        pageView(GA_MEASUREMENT_ID, url);

    }, [pathname, searchParams, GA_MEASUREMENT_ID]);

    return (
        <>

            {/* Tag Manager */}

            <Script

                src={`https://www.googletagmanager.com/gtag/js?id=${GA_MEASUREMENT_ID}`}

            />

            {/* Google Analytics */}

            <Script id="google-analytics" strategy="afterInteractive"

                dangerouslySetInnerHTML={
                    {
                        __html: `
window.dataLayer = window.dataLayer || [];

function gtag(){dataLayer.push(arguments)}

gtag('js', new Date());

gtag('consent', 'default', {
    'analytics_storage': 'denied'
});

gtag('config', '${GA_MEASUREMENT_ID}',{page_path: window.location.pathname,});

`,
                    }
                }

            />
        </>

    )
}

export default GoogleAnalytics
```


### 2-Create a new helper function in the lib folder gtaghelper.js

```js
// Google tag manager Helper Function

export const pageView = (GA_MEASUREMENT_ID, url) => {
  window.gtag("config", GA_MEASUREMENT_ID, {
    page_path: url,
  });
};

```

### 3-In the main layout.jsx

1. Import the Google Analytics Component 

2. Add the Google Analytics Component above the **body** and under the **HTML**

3. pass the Google Analytics ID to the component "GA_MEASUREMENT_ID" prop, for example "G-NV7VYCW493"

```js
import GoogleAnalytics from "./components/GoogleAnalytics/GoogleAnalytics";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <GoogleAnalytics GA_MEASUREMENT_ID={"G-NV7VYCW493"} />
      <body className={inter.className}>{children}</body>
    </html>
  );
}


```


