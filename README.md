# 우아한 티켓팅

## プロジェクト紹介 

リソースが限られた状況でも大量のトラフィックに耐えられる、安定的なチケット予約サービスです。

## プロジェクト目標

### コア目標

- 座席の先取りが同時に行われても安全なチケットシステムを設計
- ページを更新しても順番が維持される親切な待機列システムを設計

### 付加目標

- ユーザー体験を向上させるために、リアルタイムでの座席先取り状況をクライアントに反映

## 👨‍👩‍👧‍👦 チームメンバー紹介

<table>
    <tr align="center">
        <td><B>Backend</B></td>
        <td><B>Backend</B></td>
        <td><B>Backend</B></td>
        <td><B>Backend</B></td>
    </tr>
    <tr align="center">
        <td><a href="https://github.com/hseong3243">박혜성</a></td>
        <td><a href="https://github.com/seminchoi">최세민</a></td>
        <td><a href="https://github.com/mirageoasis">김현우</a></td>
        <td><a href="https://github.com/lass9436">이영민</a></td>
    </tr>
    <tr align="center">
        <td>
            <img src="https://github.com/hseong3243.png?size=130">
        </td>
        <td>
            <img src="https://github.com/seminchoi.png?size=100">
        </td>
        <td>
            <img src="https://github.com/mirageoasis.png" width="100">
        </td>
        <td>
            <img src="https://github.com/lass9436.png?size=100">
        </td>
    </tr>
</table>

## 使用技術

<div> 
  <img src="https://img.shields.io/badge/java-007396?style=for-the-badge&logo=java&logoColor=white">
  <img src="https://img.shields.io/badge/gradle-02303A?style=for-the-badge&logo=gradle&logoColor=white">
  <img src="https://img.shields.io/badge/springboot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white">
  <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
  <img src="https://img.shields.io/badge/redis-FF4438?style=for-the-badge&logo=redis&logoColor=white">
  <br/>

  <img src="https://img.shields.io/badge/grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white">
  <img src="https://img.shields.io/badge/prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white">
  <img src="https://img.shields.io/badge/locust-3ECC5F?style=for-the-badge&logo=locust&logoColor=white">
  <br/>

  <img src="https://img.shields.io/badge/aws-232F3E?style=for-the-badge&logo=amazonwebservices&logoColor=white">
  <img src="https://img.shields.io/badge/ec2-FF9900?style=for-the-badge&logo=amazonec2&logoColor=white">
  <img src="https://img.shields.io/badge/rds-527FFF?style=for-the-badge&logo=amazonrds&logoColor=white">
  <img src="https://img.shields.io/badge/alb-8C4FFF?style=for-the-badge&logo=awselasticloadbalancing&logoColor=white">
  <br/>
</div>

## インフラ構成

![인프라 구성 excalidraw](https://github.com/user-attachments/assets/06d872ef-eabe-4747-87a8-063ff0fa0e88)

## 解決した課題

### ペアプログラミングによる座席先取りの同時性制御方式の比較と選定

- 座席の先取りのために最適なロック方式を見つけるために楽観ロック、悲観ロック、分散ロックを実装
- 以下のテスト条件に従って負荷テストを実施
- 結果により平均応答時間が最も短かった悲観ロックを選定

<img width="1348" alt="스크린샷 2024-08-31 오후 10 53 58" src="https://github.com/user-attachments/assets/26b12da6-1431-4a93-8b72-aceaba721752">

### ペアプログラミングによる待機列システム設計

#### 基本設計

![대기열 시스템 구성](https://github.com/user-attachments/assets/4a9b35a2-1c39-47d2-af63-a23b3e649a21)

#### 問題1. いつユーザーを待機列から作業空間へ移動させるか？

![대기열시스템_문제1](https://github.com/user-attachments/assets/dc04c956-c1da-4ff0-bd01-9c63f0a4b873)

#### 問題2. 離脱したユーザーとアクティブなユーザーをどう区別するか？

![대기열 시스템 문제2](https://github.com/user-attachments/assets/3d78c90a-01f3-45f8-89b5-b9443a8362b9)

### 1万人に耐えられる待機列システムの検証

- 以下のテスト構成に従って負荷テストを実施
- 仮想ユーザー2,500人のとき、ほとんどのリクエストが1秒以内に処理されたことを確認
- 最終的に仮想ユーザー1万人、`残りの順番確認`のポーリング周期5秒のとき、システムが安定して動作することを検証

<img width="1132" alt="스크린샷 2024-08-31 오후 10 09 15" src="https://github.com/user-attachments/assets/45cd32c0-4f4c-4730-83be-11ddbaa1a4e9">

<img width="1207" alt="스크린샷 2024-08-31 오후 10 18 01" src="https://github.com/user-attachments/assets/815c077e-ab7b-4dde-bc64-0645e02b0b56">

![최종 검증](https://github.com/user-attachments/assets/493831f5-01b4-49be-930d-33887efe0204)

### 負荷テストに基づく待機列ページの応答時間改善

- LocustのRPSと応答時間グラフに異常を検知
- GrafanaのモニタリングでHeapメモリ使用量と異常なポイントが一致することを確認
- GC頻度を減らすためにロジックを改善
- 仮想ユーザー2,500人、テスト時間15分、`残りの順番確認API`を1秒ごとにポーリングして検証

![응답 시간 개선 excalidraw](https://github.com/user-attachments/assets/71cf70a0-622e-4177-9a89-6309e503f053)

<img width="1448" alt="응답 시간 개선2" src="https://github.com/user-attachments/assets/50314777-bc30-4410-91d3-83f2e314dfd0">

## デモ

### 待機列画面
<img src="https://github.com/user-attachments/assets/7420a4f7-e5f3-40f6-ad5d-02c14739b4e7" width="444" height="507"/>

### 座席選択画面
<img src="https://github.com/user-attachments/assets/87a1ed80-2a65-4836-a0be-f75dc9fccdcf" width="444" height="507"/>

### 全体シナリオ  
https://github.com/user-attachments/assets/eb29c948-4a1c-41fd-a7b5-b6f4d167969c

## ERD
![스키마](https://github.com/user-attachments/assets/cc43dfd9-3135-47a4-b584-5e8184f1024d)

## 📜 グラウンドルール

- スクラムと振り返りは10分以内で行う。
- 火曜日は残業デー！21時までコーディングします。
- 予定がある場合は事前に共有する。
- 平日10時〜22時まではオンコールタイム（コアタイム）とする。
- オンコールタイム中のプロジェクト関連のやりとりは **Discord** で行う。
    - 要件は一度に6W1Hに従って伝える。
    - DMは使わず、すべての問題を公開的に共有する。
- 質問はたくさん、自由に行う。
- その週に終わらなかった開発は週末に仕上げる。それ以外の週末は自由に使ってよい。
- 批判はしても、感情を害するような非難はしない。

## 🚷 開発コンベンション

https://github.com/woowa-techcamp-2024/Team3-rdParty/wiki/%EC%BB%A8%EB%B2%A4%EC%85%98
