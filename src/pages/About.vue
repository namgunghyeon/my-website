<template>
  <Layout>
    <main>
      <header class="max-w-xl md:max-w-2xl xl:max-w-3xl mx-auto text-center px-6 pt-24">
        <h1 class="text-4xl sm:text-5xl md:text-6xl font-sans font-bold mb-1">남궁현</h1>
      </header>
      <nav class="absolute top-0 left-0 z-20 mt-6 ml-6">
        <g-link to="/" class="text-gray-900 border-gray-400 text-sm border opacity-75 hover:opacity-100 rounded-full px-4 py-2 transition-opacity">&larr; Home</g-link>
      </nav>
      <article class="max-w-xl md:max-w-2xl xl:max-w-3xl mx-auto px-6 sm:px-12 pt-16 border-b border-gray-300 pb-10 mb-16">


        <div class="markdown text-lg leading-normal text-gray-700">
          <p class="text-1xl font-bold">좋은 구조로 개발하고 유지 보수를 쉽게 할 수 있는지 고민하고 실행하는 개발자이며, 작성한 코드에는 테스트를 넣어 신뢰성과 유지보수를 쉽게 하고 싶어합니다.</p>

          <h2>Work Experience</h2>

          <div class="markdown text-lg leading-normal text-gray-700">
            <div class="grid grid-cols-3 gap-2">
              <div class="col-span-1">
                <h1 class="text-2xl font-bold">
                  바이럴픽
                </h1>
                <div>Server Developer</div>
                <p>2021. 08 ~ 현재</p>

              </div>
              <div class="col-span-2 overflow-auto">
                <div>
                  <h1 class="text-2xl font-bold">
                    바이럴픽 해외 소싱 e-Commerce
                  </h1>
                  <p>2021. 08 ~ </p>
                  <div class="text-1xl font-bold">Description</div>
                  <p>해외 1688 쇼핑몰과 연동해 국내 도매 사용자에게 해외 소싱을 쉽게 도와주는 e-Commerce를 개발 및 유지보수 하고 있습니다.</p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>주문, 배송, 내부 관리 툴 ERD 설계</li>
                  <li>Payment, Delivery, Order API 개발</li>
                  <li>배송, 환율 내부에 필요한 Batch 개발</li>
                  <li>내부 관리에 필요한 시스템 개발</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>AWS ECS, AWS SQS, Spring Boot, JPA, Mysql, Redis</li>
                </div>
              </div>
            </div>

            <hr>

            <div class="grid grid-cols-3 gap-2">
              <div class="col-span-1">
                <h1 class="text-2xl font-bold">
                  현대카드
                </h1>
                <div>Server Developer</div>
                <p>2020. 01 ~ 2021. 08</p>

              </div>
              <div class="col-span-2 overflow-auto">
                <div>
                  <h1 class="text-2xl font-bold">
                    현대카드 앱 혜택 추천 API
                  </h1>
                  <p>2020. 04 ~ 2021. 08</p>
                  <div class="text-1xl font-bold">Description</div>
                  <p>현대카드 앱에서 사용자 기반 추천 내용을 제공하는 API 개발과 운영을 담당했습니다.</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>현대카드 앱에서 사용자 기반 혜택 추천 API 개발</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>Spring Boot, Mysql, Couchbase, Redis </li>
                </div>
                <p> </p>
                <br>
                <div>
                  <h1 class="text-2xl font-bold">
                    현대카드 BigData Platform 운영
                  </h1>
                  <p>2020. 01 ~ 2021. 08</p>
                  <div class="text-1xl font-bold">Description</div>
                  <p>현대카드에 운영하고 있는 BigData Platform을 운영했습니다.</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>내부 사용자가 HUE에 접속할 수 있도록 내부 시스템과 연동해 OAuth2.0 기반으로 인증 시스템 개발</li>
                  <li>내부 사용자 관리</li>
                  <li>시스템 모니터링 및 유지보수</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>Cloudera, Hadoop Ecosystem</li>
                </div>
              </div>
            </div>

            <hr>

            <div class="grid grid-cols-3 gap-2">
              <div class="col-span-1">
                <h1 class="text-2xl font-bold">
                  레코벨
                </h1>
                <div>Server Developer</div>
                <p>2018. 04 ~ 2019. 12</p>

              </div>
              <div class="col-span-2 overflow-auto">
                <div>
                  <h1 class="text-2xl font-bold">
                    프리비아 여행 추천 플랫폼
                  </h1>
                  <div class="text-1xl font-bold">Description</div>
                  <p>타이드스퀘가 제공하는 프리비아 여행 서비스에 추천 서비스를 위한 플랫폼 개발</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>사용자 행동 이력 데이터를 받는 Receiver API 개발</li>
                  <li>여행 상품 추천 API 개발</li>
                  <li>Kinesis to S3로 전달하는 데이터 파이프라인 구성</li>
                  <li>AWS Function Step으로 추천 Batch Flow 구성</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>AWS Kinesis, Function Step, EB, Spring Boot</li>
                </div>
                <p> </p>
                <br>
                <div>
                  <h1 class="text-2xl font-bold">
                    스타벅스 앱 음료 추천 API
                  </h1>
                  <div class="text-1xl font-bold">Description</div>
                  <p>스타벅스 앱에서 사용자에게 맞는 음료 추천을 제공하는 API 개발 및 추천 Batch가 동작할 수 있도록 시스템 구성</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>주문 데이터 수집를 위해 Fluentd를 사용해 Data Collector 서버 구축</li>
                  <li>추천 및 결과 Batch를 위한 Azkaban 환경 구축</li>
                  <li>음료 추천 API 개발</li>
                  <li>추천 결과를 확인하기 위한 Dashboard 개발</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>Spring Boot, Postgresql, Azkaban, Fluentd, Vue</li>
                </div>
                <p> </p>
                <br>
                <div>
                  <h1 class="text-2xl font-bold">
                    레코벨 추천 플랫폼
                  </h1>
                  <div class="text-1xl font-bold">Description</div>
                  <p>고객사의 데이터를 수집해 고객사 별로 추천 서비스를 제공할 수 있는 추천 플랫폼을 개발했습니다.</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>고객사 데이터를 받는 Data Receiver API 개발</li>
                  <li>Kinesis를 사용한 데이터 집계 및 데이터를 저장하는 Merge API 개발</li>
                  <li>추천 및 내부 리포트 Batch를 위한 Azkaban 운영</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>AWS S3, RedShift, Mysql, Kinesis, Spring Boot</li>
                </div>
              </div>
            </div>

            <hr>

            <div class="grid grid-cols-3 gap-2">
              <div class="col-span-1">
                <h1 class="text-2xl font-bold">
                  인코어드
                </h1>
                <div>Fullstack Developer</div>
                <p>2014. 11 ~ 2018. 03</p>

              </div>
              <div class="col-span-2 overflow-auto">
                <div>
                  <h1 class="text-2xl font-bold">
                    애너톡 DR(Demand and Response) 플랫폼
                  </h1>
                  <div class="text-1xl font-bold">Description</div>
                  <p>에너톡을 사용하고 있는 사용자에게 전기 사용량 감축을 요청하고 진행 상태를 확인하는 플랫폼을 개발했습니다.</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>DR API 개발</li>
                  <li>전기 사용량 감축 대상 통보 Batch 개발</li>
                  <li>DR 진행 상태 확인 Batch</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>AWS Mysql, S3, NodeJs, Python</li>
                </div>
                <p> </p>
                <br>
                <div>
                  <h1 class="text-2xl font-bold">
                    에너톡 앱 API
                  </h1>
                  <div class="text-1xl font-bold">Description</div>
                  <p>에너톡 앱에서 사용하는 API를 개발 했습니다.</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>APP에서 사용하는 API 개발</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>AWS Mysql, EB, Cassandra, NodeJS</li>
                </div>
                <p> </p>
                <br>
                <div>
                  <h1 class="text-2xl font-bold">
                    내부 관리 시스템
                  </h1>
                  <div class="text-1xl font-bold">Description</div>
                  <p>내부에서 에너톡 관련된 서비스를 제어할 수 있도록 관리 시스템 개발을 개발했습니다.</p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>Internal API 개발</li>
                  <li>App Push API 개발</li>
                  <li>AngularJS로 작성된 코드 React로 전환</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>AngularJS, React, NodeJS, Python</li>
                </div>
              </div>
            </div>
          </div>

          <h2>Skills</h2>
          <h2>Personal Projects</h2>
          <div class="markdown text-lg leading-normal text-gray-700">
            <div class="grid grid-cols-3 gap-2">
              <div class="col-span-1">
                <h1 class="text-2xl font-bold">
                  썸어라운드 <br>운동 기록 앱
                </h1>
                <p>2021. 11 ~ 현재</p>
              </div>
              <div class="col-span-2 overflow-auto">
                <div>
                  <h1 class="text-2xl font-bold">
                    운동을 계획하고 기록하는 앱
                  </h1>
                  <a href="https://apps.apple.com/kr/app/%EC%8D%B8%EC%96%B4%EB%9D%BC%EC%9A%B4%EB%93%9C-%EC%9A%B4%EB%8F%99-%EA%B8%B0%EB%A1%9D/id1538255500" target="_blank"> 썸어라운드 운동 기록 앱</a>
                  <p>2021. 08 ~ </p>
                  <div class="text-1xl font-bold">Description</div>
                  <p>운동 기록은 운동 프로그램을 미리 작성하고 기록하는 앱입니다. 어제 몇 개 했지?? 어제보다 덜 했나? 어제보다 많이 했나? 헷갈린 적 있지 않나요? 이런 생각이 만들어 직접 사용햐려고 만든 앱입니다.</p>
                  <p> </p>
                  <div class="text-1xl font-bold">What did I do.</div>
                  <li>APP 개발</li>
                  <li>API 개발</li>
                  <p> </p>
                  <div class="text-1xl font-bold">Tech Stack.</div>
                  <li>AWS Mysql, EB, Spring Boot, React Native</li>
                </div>
              </div>
            </div>
          </div>
        </div>
      </article>
      <site-footer class="pb-8 sm:pb-10" />
    </main>
  </Layout>
</template>

<script>
import config from '~/.temp/config.js'
import SiteFooter from '@/components/Footer'

export default {
  components: {
    SiteFooter
  },
  metaInfo () {
    return {
      title: 'About',
      meta: [
        {
          key: 'description',
          name: 'description',
          content: 'Introduction to the Bleda blog starter for Gridsome.'
        },

        { property: "og:type", content: 'article' },
        { property: "og:title", content:'About' },
        { property: "og:description", content: 'Introduction to the Bleda blog starter for Gridsome.' },
        { property: "og:url", content: `${this.config.siteUrl}/about/` },
        { property: "og:image", content: '/images/bleda-card.png' },

        { name: "twitter:card", content: "summary_large_image" },
        { name: "twitter:title", content: 'About' },
        { name: "twitter:description", content: 'Introduction to the Bleda blog starter for Gridsome.' },
        { name: "twitter:site", content: "@cossssmin" },
        { name: "twitter:creator", content: "@cossssmin" },
        { name: "twitter:image", content: '/images/bleda-card.png' },
      ],
    }
  },
  computed: {
    config () {
      return config
    },
  },
}
</script>
