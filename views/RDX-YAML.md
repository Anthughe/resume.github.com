openapi: 3.0.1
info:
  title: VCAS RDX API
  description: This document provides a user-friendly file format for viewing and
    implementing VCAS RDX API requests and responses including Risk, Stepup, InitateAction
    and Validate. For best results, this file should be viewed using Swagger or similar
    tool to render the yaml file. This document is designed to be used in conjunction
    with the VCAS Real-Time Data Exchange specification.
  contact:
    name: Visa Consumer Authentication Service
    url: https://corporate.visa.com/en/products/consumer-authentication-service.html
  version: 2.2.4
x-readme:
  proxy-enabled: false
servers:
  - url: https://4f1f4a08-8aba-4366-bbcc-001af05920ab.mock.pstmn.io
paths:
  /risk:
    post:
      tags:
        - RDX Requests
      summary: Risk Request
      description: Risk-based authentication requests are sent by VCAS to the partner. The partner receives the request and responds with Success, Failure or Stepup.
      operationId: risk
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RiskRequest'
        description: Risk Request Object
        required: true
      responses:
        '200':
          description: Successful Risk Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RiskResponse'
        '405':
          description: Invalid input
  /stepup:
    post:
      tags:
        - RDX Requests
      summary: Stepup Request, Biometric
      description: A Stepup Request is communicated by the VCAS platform to the partner. The partner responds with parameters necessary for VCAS to perform the step-up challenge.
      operationId: stepup-biometric
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/StepupRequest'
        description: Stepup Request Object
        required: true
      responses:
        '200':
          description: Successful Stepup Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/StepupResponse'
        '405':
          description: Invalid input
  /initiateaction:
    post:
      tags:
      - RDX Requests
      summary: Initiate Action Request
      description: The InitiateAction request is used to signal to the partner to
        take action on an item.
      operationId: initiateaction
      requestBody:
        description: InitiateAction Request Object
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/InitiateActionRequest'
        required: true
      responses:
        200:
          description: Successful Initiate Action Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/InitiateActionResponse'
        405:
          description: Invalid input
          content: {}
      x-codegen-request-body-name: body
  /validate:
    post:
      tags:
      - RDX Requests
      summary: Validate Request
      description: The Validate request is communicated by the VCAS platform to the
        partner. The partner responds with a success, failure or retry logic.
      operationId: validate
      requestBody:
        description: Stepup Validation Request Object
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ValidateRequest'
        required: true
      responses:
        200:
          description: Successful Validate Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ValidateResponse'
        405:
          description: Invalid input
          content: {}
      x-codegen-request-body-name: body
  /OOBPushValidateStatus:
    post:
      tags:
        - RDX Requests
      summary: Validate Request for OOB CallBack Status API
      description: Following successful transaction authentication, the authentication outcome will be sent to VCAS via the OOBCallBackValidateStatus API.
      operationId: OOBCallBackValidateStatus
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/OOBCallBackValidateStatus'
        description: OOB CallBack Validate Status Request
        required: true
      responses:
        '200':
          description: Successful OOB CallBack Validate Status Request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OOBCallBackValidateStatus'
        '400':
          description: Bad Request - Invalid input
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OOBCallBackValidateStatusError-400'
        '401':
          description: Unauthorized - Invalid or missing authentication
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OOBCallBackValidateStatusError-401'
        '404':
          description: Not Found - Resource not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OOBCallBackValidateStatusError-404'
        '500':
          description: Internal Server Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OOBCallBackValidateStatusError-500'
components:
  schemas:
    MerchantInfo:
      required:
      - MerchantURL
      type: object
      properties:
        AcquirerId:
          type: string
          maxLength: 11
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: AcquirerId for the merchant performing the purchase request.
            Up to 11 characters.
          example: "1337"
        AcquirerCountryCode:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Country code of the Acquirer. ISO 3166-1 numeric format. Issuers
            need to be aware of the acquirer country code when the acquirer country
            differs from the merchant country and the acquirer is in the EEA (this
            could mean that the transaction is covered by PSD2). Note: Currently only
            available on Mastercard EMV 3DS transactions where extension data is present.'
          example: "840"
        MerchantId:
          type: string
          maxLength: 35
          minLength: 35
          pattern: '^.{35}$'
          description: MerchantId for the merchant performing the purchase request.
          example: "876543210"
        MerchantName:
          type: string
          maxLength: 40
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Merchant Name for the merchant performing the purchase request.
            Max. 40 characters.
          example: Ranier Expeditions
        MerchantURL:
          type: string
          maxLength: 2048
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: URL or App Name for the merchant's website or app. Max. 2048
            characters.
          example: https://www.requestor.com
        MerchantCategoryCode:
          type: string
          maxLength: 4
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Code used to describe the merchant's type of business product
            or service.'
          example: "0123"
        MerchantCountryCode:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Country code of the merchant. For 3DS1 transactions this value
            is alpha-2 format e.g. US. For 3DS2 transactions this value is numeric-3
            format e.g. 840.
          example: "840"
    MerchantAppRedirectURLInfo:
      required:
      - MerchantURL
      type: object
      properties:
        AcquirerId:
          type: string
          maxLength: 11
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: AcquirerId for the merchant performing the purchase request.
            Up to 11 characters.
          example: "1337"
        AcquirerCountryCode:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Country code of the Acquirer. ISO 3166-1 numeric format. Issuers
            need to be aware of the acquirer country code when the acquirer country
            differs from the merchant country and the acquirer is in the EEA (this
            could mean that the transaction is covered by PSD2). Note: Currently only
            available on Mastercard EMV 3DS transactions where extension data is present.'
          example: "840"
        MerchantId:
          type: string
          maxLength: 35
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: MerchantId for the merchant performing the purchase request.
          example: "987654321"
        MerchantName:
          type: string
          maxLength: 40
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Merchant Name for the merchant performing the purchase request.
            Max. 40 characters.
          example: Ranier Expeditions
        MerchantURL:
          type: string
          maxLength: 2048
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: URL or App Name for the merchant's website or app. Max. 2048
            characters.
          example: https://www.requestor.com
        MerchantCategoryCode:
          type: string
          maxLength: 4
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Code used to describe the merchant's type of business product
            or service.
          example: "0123"
        MerchantCountryCode:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Country code of the merchant. For 3DS1 transactions this value
            is alpha-2 format e.g. US. For 3DS2 transactions this value is numeric-3
            format e.g. 840.
          example: "840"
        MerchantAppRedirectURL:
          type: string
          maxLength: 256
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Merchant app declaring their URL within the CReq message so
            that the Authentication app can call the Merchant app after OOB authentication
            has occurred. Each transaction requires a unique Transaction ID by using
            the SDK Transaction ID. VCAS will validate the value to ensure it contains
            a scheme which will route the authentication app to the associated merchant
            app. Example: merchantScheme://appName?transID=b2385523-a66c-4907-ac3c-91848e8c0067'
          example: "merchantScheme://appName?transID=b2385523-a66c-4907-ac3c-91848e8c0067"
    PaymentInfo:
      required:
      - CardExpiryMonth
      - CardExpiryYear
      - CardNumber
      type: object
      properties:
        CardNumber:
          type: string
          maxLength: 19
          minLength: 13
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Payment card number used in transaction. Length - between 13
            and 19 characters.
          example: "4012009500714811"
        CardExpiryMonth:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Expiration month
          example: "08"
        CardExpiryYear:
          type: string
          maxLength: 4
          minLength: 4
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Expiration year. For 3DS1 transactions this value is 4-digits
            e.g. 2028. For 3DS2 transactions this value is 2-digits e.g. 23.
          example: "28"
        CardType:
          type: string
          maxLength: 5
          minLength: 13
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Card or account type e.g. Debit or Credit.
          enum:
          - Credit
          - Debit
          - NotApplicable
        CardHolderName:
          type: string
          maxLength: 45
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Name of the cardholder. Max. 45 characters.
          example: Jane Doe
    Address:
      required:
      - FirstName
      - LastName
      type: object
      properties:
        FirstName:
          type: string
          maxLength: 64
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: First Name for the Address Component.
        MiddleName:
          type: string
          maxLength: 64
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Middle Name for the Address Component.
        LastName:
          type: string
          maxLength: 64
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Last Name for the Address Component.
        Address1:
          type: string
          maxLength: 128
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Address Line 1.
        Address2:
          type: string
          maxLength: 128
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Address Line 2.
        Address3:
          type: string
          maxLength: 128
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Address Line 3.
        Locality:
          type: string
          maxLength: 128
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: City, Town, etc.
        Region:
          type: string
          maxLength: 64
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: State, Province, Territory, etc.
        PostalCode:
          type: string
          maxLength: 32
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Postal Code.
        CountryCode:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Country Code Format will follow [ISO 3166-1 three digit numeric]
            3 characters.
    CardBrandName:
      type: string
      maxLength: 10
      minLength: 3
      pattern: '^(0[1-9]|[1-9][0-9])$'
      description: Name of the card network for a specific transaction.
      enum:
          - VISA
          - DISCOVER
          - MASTERCARD
          - JCB
          - AMEX
          - DINERS
          - ELO
          - UPI
          - EFTPOS
          - MADA
          - MYDEBIT
          - TAKAPAY
          - ITMX
          - JAYWAN
      example: 'VISA'
    ConsumerContact:
      type: object
      properties:
        EmailAddress:
          type: string
          maxLength: 254
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Email address for the consumer. Max. 254 characters.
          format: email
        PhoneNumber:
          type: string
          maxLength: 15
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Phone number for the consumer. Max. 15 characters.
        MobileNumber:
          type: string
          maxLength: 15
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Consumer's mobile number. Max. 15 characters.
        WorkNumber:
          type: string
          maxLength: 15
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Work phone number for the cardholder. Max. 15 characters.
    TransactionType:
      description: Name of the card network for a specific transaction.
      enum:
          - GoodsOrService
          - CheckAcceptance
          - AccountFunding
          - QuasiCash
          - PrepaidActivation
      example: 'GoodsOrService'
    WalletInfo:
      type: object
      properties:
        Provider:
          type: string
          maxLength: 100
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Wallet provider name. Max. 100 characters.
        WalletAge:
          maximum: 1E+4; 1000
          minimum: 2
          type: number
          description: Number of days since the wallet was created.
          format: Int64
        PaymentCardAge:
          maximum: 1E+4; 1000
          minimum: 2
          type: number
          description: Number of days the card has been in the wallet.
          format: Int64
    MerchantAdditionalData:
      type: object
      properties:
        ShippingIndicator:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates shipping method chosen for transaction.
          enum:
          - ShipToBillingAddress
          - ShipToVerifiedAddress
          - ShipToOtherAddress
          - ShipToStore
          - DigitalGoods
          - TravelOrEventTickets
          - Other
        DeliveryTimeFrame:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Deilvery timeframe.
          enum:
          - ElectronicDelivery
          - SameDayShipping
          - OvernightShipping
          - TwoOrMoreDaysShipping
        DeliveryEmailAddress:
          type: string
          maxLength: 254
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Email address where merchandise was delivered. Max. 254 characters.
        ReorderItemsIndicator:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates whether cardholder ordered previously.
          enum:
          - FirstTime
          - Reordered
        PreorderPurchaseIndicator:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates purchase with future availability.
          enum:
          - MerchandiseAvailable
          - FutureAvailability
        PreorderDate:
          type: string
          maxLength: 8
          minLength: 8
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Expected date merchandise is available. Format - YYYYMMDD.
            Length - 8 characters.
        GiftCardAmount:
          type: number
          maxLength: 15
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: For a gift card, this is the purchase amount (represented in
            raw amount, example 1000 for $10.00). Max. 15 characters.
        GiftCardCurrency:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ISO 4217 3-digit numeric standard.[ISO 4217 Link] https://en.wikipedia.org/wiki/ISO_4217
            Length - 3 characters.
        GiftCardCount:
          type: number
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Total count of individual prepaid or gift cards/codes purchased.
            Length - 2 characters.
    DeviceInfo:
      type: object
      properties:
        UserAgent:
          type: string
          maxLength: 2048
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: User Agent for browser or Device Identifier for InApp purchase.
            Max. 2048 characters.
        IP:
          type: string
          maxLength: 45
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: IP Address of the device. Max. 45 characters.
        Latitude:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Latitude of the device based on geolocation or IP Address.
            Max. 50 characters.
        Longitude:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Longitude of the device based on geolocation or IP Address.
            Max. 50 characters.
        BrowserAcceptHeader:
          type: string
          maxLength: 2048
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Raw HTTP Accept header from the browser. Max. 2048 characters.
        BrowserJavaEnabled:
          type: string
          maxLength: 5
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates whether browser can execute Java. Example, true.
            Max. 5 characters.
        BrowserJavascriptEnabled:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates whether browser can execute Javascript. Example,
            true. Max. 5 characters.
        BrowserLanguage:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Browser language returned from navigator language property.
            Max. 8 characters.
        BrowserColorDepth:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Value representing the bit depth of the color palette. Max.
            2 characters.
        BrowserScreenHeight:
          type: string
          maxLength: 6
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Height of cardholder's screen in pixels. Max. 6 characters.
        BrowserWidth:
          type: string
          maxLength: 6
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Width of cardholder's screen in pixels. Max. 6 characters.
        BrowserTimeZone:
          type: string
          maxLength: 5
          minLength: 5
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Time difference between UTC time and the cardholder's browser
            local time, in minutes. From Date.getTimezoneOffset() method. Max. 5 characters.
        IPCountry:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Country of origin from IP address. Available only in browser-initiated
            transactions when the Method URL successfully completes. Length varies.
        Platform:
          type: string
          maxLength: 30
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Platform of the device. Example, Android, iOS. Max. 30 characters.
        DeviceModel:
          type: string
          maxLength: 100
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Mobile device manufacture and model. Max. 25 characters.
        DeviceId:
          type: string
          maxLength: 100
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Risk Provider device identifier or fingerprint. Max. 100 characters.
        OperatingSystemName:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Max. 50 characters.
        OperatingSystemVersion:
          type: string
          maxLength: 25
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Max. 25 characters.
        Locale:
          type: string
          maxLength: 2048
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Device Locale from the browser header or from the app’s language
            settings. This value can be a single locale value or multiple concatenated
            together via commas. In 3DS1 this value will be directly from the browser’s
            “Accept-Language” header. However, in 3DS2 this is not available due to
            new flows so this will be a single language value following BCP 47 format
            e.g. en-US or en,es-PE;q=0.9,es;q=0.8
        AdvertisingId:
          type: string
          maxLength: 128
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Unique ID available for advertising and fraud detection purposes.
            Max. 128 characters.
        ScreenResolution:
          type: string
          maxLength: 15
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Pixel width and height. Example, 1080x1920. Max. 15 characters.
        DeviceName:
          type: string
          maxLength: 256
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: User assigned device name. Max. 50 characters.
        SDKAppId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Universally unique ID created upon all installations and upates
            of the 3DS Requestor App on a consumer device. Length - Up 36 characters.
        DeviceExtendedData:
          type: string
          maxLength: 64000
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Base64url encoded JSON object. Device information gathered
            by 3DS SDK from a consumer device. Max. 64000'
    RiskProviderInfo:
      type: object
      properties:
        Name:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: |
            Device Profiling and Risk Engine Provider. Values are defined as follows:
            - TM - ThreatMetrix
        ProviderId:
          type: string
          maxLength: 100
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Risk Provider transaction reference Id. Max. 100 characters.
        SessionId:
          type: string
          maxLength: 100
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: The ID is the transaction session ID provided by the 3DS server. Referred to as threeDSServerTransID in the EMVCo specification.
    ExtensionData:
      type: object
      properties:
        DAFExtension:
          $ref: '#/components/schemas/DAFExtension'
        VisaPaymentTokenExtension:
          $ref: '#/components/schemas/VisaPaymentTokenExtension'
    DAFExtension:
      type: object
      properties:
        AuthPayCredStatus:
          type: string
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Enables the communication of Authenticated Payment Credential Status between the VDS and the 3DS Server, and the VDS and the ACS. (Y, N, U, B, I). One character.
          example: 'Y'
        AuthPayProcessReqInd:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates whether the purpose of the transaction is to process as a DAF transaction or to inquire on the Authenticated Payment Credential Status. 2 characters.
          example: '01'
        DafAdvice:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates to ACS whether the transaction must be approved or whether approval is an issuer decision. (01 = must approve; 02; issuer decision). 2 characters.
          example: '01'
        Version:
          type: string
          maxLength: 5
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Version number of the message extension being used; ex. 1.0. Up to 5 characters.
          example: '1.0'
    VisaPaymentTokenExtension:
      type: object
      properties:
        TokenRequestorId:
          type: string
          maxLength: 11
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: A value that identifies each unique combination of Token Requestor
            and Token Domain(s) for a given Token Service Provider. 11 characters.
          example: "12345678910"
        TokenStatusIndicator:
          type: string
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: |
            Identifies the status of the Payment Token. 1 character.
            Values are defined as:
            - A - Active for payment
            - I - Inactive for payment
            - S - Temporarily suspended for payment
            - D - Permanently deactivated for payments
            - P - Pending'
          example: 'A'
        TokenAdditionalData:
          $ref: '#/components/schemas/TokenAdditionalData'
        Version:
          type: string
          maxLength: 5
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Version number of the token message extension. Up to 5 characters.
        Token:
          type: string
          maxLength: 19
          minLength: 13
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Payment token used to initiate the EMV 3DS transaction. 13-19 characters.
        TokenAssuranceMethod:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: An updatable value that allows the Token Service Provider to communicate the ID&V performed. It is determined or updated as a result of the ID&V Method(s) and ID&V Actor. 2 characters.
        TokenCryptogram:
          type: string
          maxLength: 4000
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: A cryptogram, containing a transaction-unique value, typically generated using the Payment Token, Payment Token related data and transaction data. 4000 characters max.
        TokenCryptogramValidityIndicator:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: |
            Identifies if the Token Cryptogram has been verified and the outcome of that verification. If the element is not provided, the expected action is for the ACS to interpret as 03 (Not Performed). 2 characters. Values are defined as follows:
            - 01 - Verified
            - 02 - Failed
            - 03 - Not Performed
            - 04-79 - Reserved for EMVCo future use (values invalid until defined by EMVCo)
            - 80–99 = Reserved for DS use
          example: '01'
    TokenAdditionalData:
      type: object
      properties:
        TokenAdditionalDataVersion:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: |
            Indicates the extension version and matches the value provided by the "Version" field on the "Data" object. Used for Visa transactions only. 3 characters.
          example: "1.0"
        TokenCharacteristics:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: |
            This data element indicates the type of token. Used for Visa transactions only. 2 characters. Values are defined as follows:
            - 01 - E-commerce/Card on File
            - 02 - Secure Element
            - 03 - Cloud Based Payments
            - 05 - E-commerce Enabler
            - 06 - Pseudo Account
          example: "06"
    RiskRequestTransactionInfo:
      type: object
      properties:
        TransactionTimeStamp:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction timestamp in UTC per ISO 8601 UTC. Length - 24
            characters e.g 2024-03-21T20:55:49.000Z
          format: date-time
        TransactionAmount:
          type: number
          maxLength: 48
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction Amount (raw amount, example 1000 for $10.00). Max.
            48 characters.
          format: decimal
        TransactionAmountUSD:
          type: number
          maxLength: 48
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction Amount in USD (raw amount, example 1000 for $10.00). Max.
            48 characters.
          format: decimal
        TransactionCurrency:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ISO 4217 3-digit numeric standard.[ISO 4217 Link] https://en.wikipedia.org/wiki/ISO_4217
            Length - 3 characters.
          example: "840"
        TransactionExponent:
          type: integer
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Exponent for formatting the given currency ISO 4217 code. Length
            - One character.
        TransactionType:
          $ref: '#/components/schemas/TransactionType'
        MandatedRegion:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: A value describing the region in which mandates may apply to
            the current transaction. Added to support the new PSD2 transactions in
            the EEA. A value of EEA will denote that the transaction falls under the
            PSD2 mandates, otherwise the value will be null. Note - you must account
            for new values being added to this field over time due to new regulations
            being rolled out in other regions.
          enum:
          - EEA
          - NONE
          - UNKNOWN
        Channel:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Channel in which the transaction occurs.
          example: WEB
          enum:
          - WEB
          - APP
          - MWEB
          - THREERI
        AddressMatch:
          type: string
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Shipping address matches billing address. Y = shipping and
            billing address are the same, N = shipping and billing addresses are different.
            Length - one character.
        MerchantAdditionalData:
          $ref: '#/components/schemas/MerchantAdditionalData'
        PaymentInfo:
          $ref: '#/components/schemas/PaymentInfo'
        BillingAddress:
          $ref: '#/components/schemas/Address'
        ShippingAddress:
          $ref: '#/components/schemas/Address'
        ConsumerInfo:
          $ref: '#/components/schemas/ConsumerContact'
        ConsumerWalletInfo:
          $ref: '#/components/schemas/WalletInfo'
        DeviceInfo:
          $ref: '#/components/schemas/DeviceInfo'
        RiskProviderInfo:
          $ref: '#/components/schemas/RiskProviderInfo'
        TriggeredRuleName:
          type: string
          maxLength: 254
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Name of rule triggered during risk processing.
        RecurringInfo:
          type: object
          properties:
            RecurringFrequency:
              type: string
              maxLength: 4
              minLength: 1
              pattern: '^(0[1-9]|[1-9][0-9])$'
              description: Indicates the minimum number of days between authorizations. Up to 4 characters.
              format: string
            RecurringExpiry:
              type: string
              maxLength: 8
              minLength: 8
              pattern: '^(0[1-9]|[1-9][0-9])$'
              description: Expiration date of the card on file for the transaction; YYYY-MM-DD. 8 characters.
              format: date
        ThreeDSRequestorPriorAuthenticationInfo:
          type: object
          properties:
            threeDSReqPriorAuthData:
              type: string
              maxLength: 2048
              minLength: TBD
              pattern: '^(0[1-9]|[1-9][0-9])$'
              description: Data that documents and supports a specific authentication process. Up to 2048 characters.
              format: string
            threeDSReqPriorAuthMethod:
              type: string
              maxLength: 2
              minLength: TBD
              pattern: '^(0[1-9]|[1-9][0-9])$'
              description: Mechanism used by the Cardholder to previously authenticate to the 3DS Requestor. 2 characters.
              format: string
            threeDSReqPriorAuthTimestamp:
              type: string
              maxLength: 12
              minLength: 12
              pattern: '^(0[1-9]|[1-9][0-9])$'
              description: IDate and time in UTC of the prior cardholder authentication; YYYY-MM-DD:HH-MM. 12 characters.
              format: date
            threeDSReqPriorRef:
              type: string
              maxLength: 36
              minLength: 36
              pattern: '^(0[1-9]|[1-9][0-9])$'
              description: Provides additional information to the ACS to determine the best approach for handing a request. This data element contains an ACS Transaction ID for a prior authenticated transaction (for example, the first recurring transaction that was authenticated with the cardholder). 36 characters.
              format: string
    TransStatusReason:
      type: string
      description: Provides information on why the Transaction Status field has the specified value. 2 characters.
      format: string
    StepupRequestTransactionInfo:
      type: object
      properties:
        TransactionTimeStamp:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction timestamp in UTC per ISO 8601 UTC. Length - 24
            characters e.g 2024-03-21T20:55:49.000Z
          format: date-time
        TransactionAmount:
          type: number
          maxLength: 48
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction Amount (raw amount, example 1000 for $10.00). Max.
            48 characters.
          format: decimal
        TransactionCurrency:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ISO 4217 3-digit numeric standard.[ISO 4217 Link] https://en.wikipedia.org/wiki/ISO_4217
            Length - 3 characters.
          example: "840"
        TransactionExponent:
          type: integer
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Exponent for formatting the given currency ISO 4217 code. Length
            - one character.
        TransactionType:
          $ref: '#/components/schemas/TransactionType'
        MandatedRegion:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: A value describing the region in which mandates may apply to
            the current transaction. Added to support the new PSD2 transactions in
            the EEA. A value of EEA will denote that the transaction falls under the
            PSD2 mandates, otherwise the value will be null. Note - you must account
            for new values being added to this field over time due to new regulations
            being rolled out in other regions.
          enum:
          - EEA
          - NONE
          - UNKNOWN
        Channel:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Channel in which the transaction occurs.
          example: WEB
          enum:
          - WEB
          - APP
          - MWEB
          - THREERI
    InitiateActionTransactionInfo:
      type: object
      properties:
        TransactionTimeStamp:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction timestamp in UTC per ISO 8601 UTC. Length - 24
            characters. e.g 2024-03-21T20:55:49.000Z
          format: date-time
        TransactionAmount:
          type: number
          maxLength: 48
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction Amount (raw amount, example 1000 for $10.00). Length
            - up to 48 characters. Required for 02-NPA if 3DS Requestor Authentication
            Indicator = 02 or 03.
          format: decimal
        TransactionCurrency:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ISO 4217 3-digit numeric standard.[ISO 4217 Link] https://en.wikipedia.org/wiki/ISO_4217
            Length - 3 characters. Required for 02-NPA if 3DS Requestor Authentication
            Indicator = 02 or 03.
          example: "840"
        TransactionExponent:
          type: integer
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Exponent for formatting the given currency ISO 4217 code. Length
            - 1 character.
        TransactionType:
          $ref: '#/components/schemas/TransactionType'
        MandatedRegion:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: A value describing the region in which mandates may apply to
            the current transaction. Added to support the new PSD2 transactions in
            the EEA. A value of EEA will denote that the transaction falls under the
            PSD2 mandates, otherwise the value will be null. Note - you must account
            for new values being added to this field over time due to new regulations
            being rolled out in other regions.
          enum:
          - EEA
          - NONE
          - UNKNOWN
        Channel:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Channel in which the transaction occurs.
          example: WEB
          enum:
          - WEB
          - APP
          - MWEB
          - THREERI
    Credential:
      required:
      - Id
      - Type
      type: object
      properties:
        Id:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Identifier for the credential requested, this will be passed
            on the InitiateAction request as well as the ValidateRequest. Length will
            be 36 characters. The Id must be unique per credential object returned.
            The Id is used to distinguish the specific authenticate type in preceding
            InitiateAction and ValidateRequest calls.
        Type:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Type of the Credential used for stepup, this is meta data and
            will not be used in any business logic
          enum:
          - OTPEMAIL
          - OTPSMS
          - OTPIVR
          - KBASINGLE
          - BIOMETRIC
          - OUTOFBANDOTHER
          - OUTOFBANDTOKEN
        Text:
          type: string
          maxLength: 254
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Dynamic data to be displayed to the cardholder i.e. masked
            phone number or email address. Note: certain browser screen templates
            will only be able to show a certain number of characters before showing
            an elipsis (...). Up to 35 characters. Note: in EMV SDK flows the text
            space is limited. Up to 40 characters.'
    CredentialStepup:
      type: object
      properties:
        CustomerId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Required for behavioral biometrics flow. Length will be 36
            characters.
        Id:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Id value from the StepupResponse->Credential. Length will be
            36 characters.
        Type:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Type of the Credential used for stepup, this is meta data and
            will not be used in any business logic
          enum:
          - OTPEMAIL
          - OTPSMS
          - OTPIVR
          - KBASINGLE
          - BIOMETRIC
          - OUTOFBANDOTHER
          - OUTOFBANDTOKEN
        Text:
          type: string
          maxLength: 254
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Dynamic data to be displayed to the cardholder i.e. masked
            phone number or email address.
        Token:
          type: string
          maxLength: 1024
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Token field will be contained in the post request to the issuer
            and posted in the embedded iframe. This is only available and applicable
            for StepupType OUTOFBAND_EMBEDDED. Required when Credential.Type is OUTOFBANDTOKEN.
    CredentialValidate:
      type: object
      properties:
        Id:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Id value from the StepupResponse->Credential. Length is 36 characters.
        Type:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Type of the Credential used for stepup, this is meta data and will not be used in any business logic.
          enum:
          - OTPEMAIL
          - OTPSMS
          - OTPIVR
          - KBASINGLE
          - BIOMETRIC
          - OUTOFBANDOTHER
          - OUTOFBANDTOKEN
        Value:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Value provided by the cardholder on the Validate screen. This is used in OTP/KBA flows for the customer to enter the correct value.
    BehavioralBiometricsResult:
      type: object
      properties:
        CustomerId:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Holds issuer’s customerid for creating/building behavioral
            biometrics profile.
        Decision:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Returns the result of the behavioral biometrics decision.
        RiskScore:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Score indicating the result of the behavioral biometrics decision.
    ExemptionInfo:
      type: object
      properties:
        MerchantFraudRate:
          type: string
          maxLength: 2
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Merchant fraud rate in the EEA calculated as per PSD2 RTS.
            Note: Only Mastercard EMV 3DS transactions where extension data is present.
            Note: Mastercard will not calculate or validate the merchant fraud score.'
          example: "1"
        SecureCorporatePayment:
          type: string
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Indicates dedicated payment processes and procedures were
            used, potential secure corporate payment exemption applies. Logically
            this field should only be set to yes if the acquirer exemption field is
            blank. A merchant cannot claim both acquirer exemption and secure payment.
            However, the DS will not validate the conditions in the extension. DS
            will pass data as presented. Possible values: "Y" or "N". Note: Only Mastercard
            EMV 3DS transactions where extension data is present.'
          example: Y
        MCRiskScore:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Mastercard risk score provided on the AReq message extension.
            This field is configurable by issuers, however, issuers must request this
            feature to be enabled. Only applicable for Mastercard transactions.
          example: "123"
        WhitelistStatus:
          $ref: '#/components/schemas/WhitelistStatus'
        WhitelistStatusSource:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Indicates where the source for whitelisting request. This
            will be the initial value sent to VCAS on the authentication request.
            Note: EMV 3DS Transaction only.'
          enum:
          - Merchant
          - DS
      description: Object containing information related to EMV exemptions as related
        to the EEA PSD2 regulations.
    MerchantAuthInfo:
      type: object
      properties:
        DecoupledRequestIndicator:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Indicates whether the 3DS Requestor requests the ACS to utilise
            Decoupled Authentication and agrees to utilise Decoupled Authentication
            if the ACS confirms its use. Note: Not currently available; may be available
            at a later date.'
          enum:
          - DecoupledPreferred
          - NoDecoupledPreferred
        DecoupledMaxTime:
          type: string
          maxLength: 5
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Indicates the maximum amount of time that the 3DS Requestor
            will wait for an ACS to provide the results of Decoupled Authentication
            transaction (in minutes). Numeric values between 1 and 10080 accepted.
            Note: Not currently available; may be available at a later date.'
      description: Object containing information related to any Merchant Authentication
        information on EMV requests.
    CardholderSelectionInfo:
      type: object
      properties:
        Type:
          type: string
          maxLength: 1
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Type describing the cardholder selection e.g. Primary (P) or
            Secondary (S) cardholder.
          enum:
          - P
          - S
        Name:
          type: string
          maxLength: 45
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: The name of the selected cardholder. Only needed if Secondary
            cardholder is selected to match against.
      description: Object defining the consumers selection during the Cardholder Selection
        OTP flow. This feature is only populated and enabled for issuers supporting
        this flow.
    EmbeddedOOBResponseUrl:
      type: string
      maxLength: TBD
      minLength: TBD
      pattern: '^(0[1-9]|[1-9][0-9])$'
      description: The issuer must redirect to this URL when validation is complete,
        during an Embedded OOB challenge.
    Reason:
      type: object
      properties:
        ReasonCode:
          type: string
          maxLength: 32
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Used by the issuer for informational purposes. Max. 32 characters.
        ReasonDescription:
          type: string
          maxLength: 256
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Used by the issuer for informational purposes. Max. 256 characters.
    WhyInfo:
      type: object
      properties:
        Label:
          type: string
          maxLength: 45
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Label to be displayed to the Cardholder for the "why" information
            section.
        Text:
          type: string
          maxLength: 256
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Text provided by the issuer to be displayed to the Cardholder
            to explain why the Cardholder is being asked to perform the authentication
            task. Note: Carriage return is supported in this data element and is represented
            by an “\n”.'
      description: 'Object defining dynamic text values that can be shown to the consumer
        during the challenge flow. These fields will be mapped directly to both browser
        templates and SDK info fields. Note: this field is also configurable for SDK
        screens today via our template configuration. If this value is passed on RDX
        it will override the current configurable value.'
    ErrorMessage:
      type: object
      properties:
        ReferenceNumber:
          type: string
          maxLength: 15
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: System reference number for the error message. Max. 15 characters.
        ReasonDescription:
          type: string
          maxLength: 256
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Used by the issuer for informational purposes. Up to 256 characters.
        Description:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: A description of the error. Max. 50 characters.
        Message:
          type: string
          maxLength: 100
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: A message that will be displayed to the customer on the error
            screen. Max. 100 characters.
    RReqOverrides:
      type: object
      properties:
        AuthenticationMethod:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Required authentication method for the RReq and Visa CAVV values.
          enum:
          - SMS_OTP
          - HARDWARE_OTP
          - SOFTWARE_OTP
          - OTHER_OTP
          - KBA
          - BIOMETRIC
          - APP_LOGIN
          - OTHER
        AuthenticationAttempts:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates the number of authentication cycles attempted by
            the cardholder. Max. 2 characters.
        TransStatusReason:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Required when the transaction is not authenticated or when an error occurs in the OOB Embedded use case.
          enum:
          - CARD_AUTH_FAILED
          - EXCEEDS_FREQUENCY
          - TECHNICAL_ISSUE
        CustomerCancel:
          type: boolean
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates whether the customer cancelled the transaction. True
            if the customer clicked "Cancel", otherwise false. Required for OOB Embedded
            use case.
    3RIIndicator:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Indicates the type of 3RI request for EMV transactions.'
          enum:
          - 'RecurringTransaction'
          - 'InstallmentTransaction'
          - 'AddCard'
          - 'MaintainCardInformation'
          - 'AccountVerification'
          - 'SplitOrDelayedShipment'
          - 'TopUp'
          - 'MailOrder'
          - 'TelephoneOrder'
          - 'WhitelistStatusCheck'
          - 'OtherPayment'
    ThreeDSRequestorAuthenticationInd:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: |
            Indicates the type of Authentication request. This data element provides additional information to the ACS to determine the best approach for handling an authentication request. Values are defined as follows:
            - 01 - Payment Transaction
            - 02 - Recurring Transaction
            - 03 - Installment Transaction
            - 04 - Add Card
            - 05 - Maintain Card
            - 06 - Cardholder verification as part of EMV token ID&V
            - 07 - Billing Agreement
            - 08 - Split shipment
            - 09 - Delayed shipment
            - 10 - Split payment
            - 11-79 - Reserved for EMVCo future use (values invalid until defined by EMVCo) 
            - 80 - Reserved for Visa Payment Passkeys (Visa transactions only)
            - 81-99 - Reserved for future network program usage.
            Reserved values may be introduced by network program updates. New values will be defined and published as they become available.
          example: '01'
    WhitelistStatus:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: |
           Indicates whether or not the whitelist exemption was accepted. Should be used in conjunction with the RiskResponse.Status. Value can be left null if accepted or if not applicable to transaction. Values are defined as follows:
            - MerchantWhitelisted - 3DS Requestor is whitelisted by the cardholder
            - MerchantNotWhitelisted - 3DS Requestor is not whitelisted by the cardholder
    StepupType:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Used to identify the method of Stepup.
          enum:
          - CHOICE
          - OTP
          - KBA
          - BIOMETRIC
          - OUTOFBAND
          - OTP_AND_KBA
          - OTP_CHOICE_AND_KBA
          - OUTOFBAND_EMBEDDED
    RiskRequest:
      required:
      - IssuerId
      - MerchantInfo
      - MessageVersion
      - ProcessorId
      - TransactionId
      - TransactionInfo
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab080
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS generated transaction reference id. Max. 36 characters.
            GUID format.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        DSTransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Directory Server (DS) generated transaction reference id. GUID
            format.
          example: 521fa021-4791-4579-a3e9-76de87c219c0
        MerchantChallengeIndicator:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Indicates whether a challenge is requested from the merchant. Note: Please allow for future values in this field. EMV has reserved up to 99 values for future use.'
          enum:
          - 'NoPreference'
          - 'NoChallenge'
          - 'PreferChallenge'
          - 'MandatedChallenge'
          - 'NoChallengeRiskPerformed'
          - 'NoChallengeDataOnly'
          - 'NoChallengeSCAPerformed'
          - 'NoChallengeWhitelistExempt'
          - 'PreferChallengeWhitelistPrompt'
        3RIIndicator:
            $ref: '#/components/schemas/3RIIndicator'
        ThreeDSRequestorAuthenticationInd:
          $ref: '#/components/schemas/ThreeDSRequestorAuthenticationInd'
        MessageVersion:
          type: string
          maxLength: 8
          minLength: TBD
          description: Version of the message based on 3DS spec. Examples - 2.2.0,
            2.2.0
          example: 2.2.0
        RDXMessageVersion:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Version of the RDX protocol being used. This will be used to enable new features without breaking current integrations. Example: 2.2.3 and 2.2.4'
          example: 2.2.4
        MessageCategory:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Identifies the category of the message for a specific use case; 01=PA, 02=NPA
          example: '01'
        RiskScore:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Risk score of the transaction as determined by VCAS. Max. 2 characters.
        RuleOutcome:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: If the transaction is configured to evaluate risk rules, this
            will be the outcome of that evaluation.
          enum:
          - Success
          - Fail
          - FailWithFeedback
          - Challenge
          - Rejected
          - Error
          example: Success
        CardBrandName:
            $ref: '#/components/schemas/CardBrandName'
        ExemptionInfo:
          $ref: '#/components/schemas/ExemptionInfo'
        MerchantAuthInfo:
          $ref: '#/components/schemas/MerchantAuthInfo'
        MerchantInfo:
          $ref: '#/components/schemas/MerchantInfo'
        TransactionInfo:
          $ref: '#/components/schemas/RiskRequestTransactionInfo'
        ExtensionData:
          $ref: '#/components/schemas/ExtensionData'
    RiskResponse:
      required:
      - IssuerId
      - ProcessorId
      - Status
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Risk Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Risk Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab081
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Risk Request. Max. 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        Status:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Status of Risk Request.
          enum:
          - SUCCESS
          - STEPUP
          - FAILURE
          - FAILWITHFEEDBACK
          - ERROR
          - BLOCKED
          - REJECTED
        TransStatusReason:
          $ref: '#/components/schemas/TransStatusReason'
        Language:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Language to display the instructions and prompt to the cardholder.
            This value will decide which template is loaded, provided the correct
            template language is available. If not provided, the language is selected
            based on issuer configuration and browser preference e.g. en-US. Max.
            50 characters.
        RiskIndicator:
          type: string
          maxLength: 3
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: "If returned, this value will be used in the creation of certain\
            \ Authentication Values (AV) for EMV 3DS. The value passed must conform\
            \ to the current AV value the issuer is using. This may also depend on\
            \ the card brand of the transaction. For example, CAVV and IAV accept\
            \ different values. \nNote: This value will be converted to the Hexadecimal\
            \ equivalent. Refer to the “VCAS Enhanced Authentication Value Support\
            \ Guide” for details on supported authentication values.\n"
        RiskScore:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Risk Score or value as determined by the partner or VCAS
            Risk Engine. Length - 2 characters.
        Reason:
          $ref: '#/components/schemas/Reason'
        Error:
          $ref: '#/components/schemas/ErrorMessage'
    StepupRequest:
      required:
      - IssuerId
      - MessageVersion
      - ProcessorId
      - StepupCounter
      - StepupRequestId
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab083
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS generated transaction reference id. Max. 36 characters.
            GUID format.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        DSTransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Directory Server (DS) generated transaction reference id. GUID
            format.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f987
        3RIIndicator:
            $ref: '#/components/schemas/3RIIndicator'
        ThreeDSRequestorAuthenticationInd:
          $ref: '#/components/schemas/ThreeDSRequestorAuthenticationInd'
        StepupRequestId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Unique identifier to identify the particular Stepup request.
            Length is 36 characters.
          example: 878f4751-4140-4881-9e4a-003e83524f22
        StepupCounter:
          type: integer
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Counter for tracking the number of Stepups. Each stepup can
            go from Stepup -> InitiateAction -> Validate. If the user requests a "resend"
            via the browser templates it will initiate another Stepup request.
        DeviceLocale:
          type: string
          maxLength: 2048
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Device Locale from the browser header or from the app’s language
            settings. This value can be a single locale value or multiple concatenated
            together via commas. In 3DS1 this value will be directly from the browser’s
            “Accept-Language” header. However, in 3DS2 this is not available due to
            new flows so this will be a single language value following BCP 47 format
            e.g. en-US or en,es-PE;q=0.9,es;q=0.8
          example: en-US
        DeviceUserAgent:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Device user agent from the browser header or the app's device
            identifier. Max. 2048 characters.
          example: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML,
            like Gecko) Chrome/87.0.4280.88 Safari/537.36
        MessageVersion:
          type: string
          maxLength: 2048
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Version of the message based on 3DS spec. Examples 2.1.0, 2.2.0
          example: 2.2.0
        RDXMessageVersion:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Version of the RDX protocol being used. This will be used to enable new features without breaking current integrations. Example: 2.2.2 and 2.2.3'
          example: 2.2.3
        MessageCategory:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Identifies the category of the message for a specific use case; 01=PA, 02=NPA
          example: 01
        StepupReason:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Used to identify the reason the Stepup request was initiated.
            Only available for Cardholder Resend, future values and scenarios may
            be added.
          enum:
          - CARDHOLDER_RESEND
        MerchantInfo:
          $ref: '#/components/schemas/MerchantAppRedirectURLInfo'
        PaymentInfo:
          $ref: '#/components/schemas/PaymentInfo'
        TransactionInfo:
          $ref: '#/components/schemas/StepupRequestTransactionInfo'
        CardBrandName:
            $ref: '#/components/schemas/CardBrandName'
        CardholderSelectionInfo:
          $ref: '#/components/schemas/CardholderSelectionInfo'
        ExtensionData:
          $ref: '#/components/schemas/ExtensionData'
        EmbeddedOOBResponseUrlInfo:
          $ref: '#/components/schemas/EmbeddedOOBResponseUrl'
    StepupResponse:
      required:
      - Credentials
      - IssuerId
      - ProcessorId
      - Status
      - StepupRequestId
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab088
        IsBbConsentRequired:
          type: boolean
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates if legal consent is required for the behavioral biometrics
            flow.
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. Max. 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        StepupRequestId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. Length - 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f321
        StepupType:
          $ref: '#/components/schemas/StepupType'
        Language:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Language to display the instructions and prompt to the cardholder.
            This value will decide which template is loaded, provided the correct
            template language is available. If not provided, the language is selected
            based on issuer configuration and browser preference e.g. en-US. Max.
            8 characters.
        Status:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ERROR is returned on any interal/technical issues. AUTHENTICATED
            will return a Y back to the merchant.
          enum:
          - SUCCESS
          - AUTHENTICATED
          - FAILURE
          - FAILWITHFEEDBACK
          - ERROR
          - BLOCKED
          - REJECTED
        TransStatusReason:
          $ref: '#/components/schemas/TransStatusReason'
        RiskIndicator:
          type: string
          maxLength: 3
          minLength: 3
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: "If returned, this value will be used in the creation of certain\
            \ Authentication Values (AV) for EMV 3DS. \nThe value passed must conform\
            \ to the current AV value the issuer is using. This may also depend on\
            \ the card brand of the transaction. For example, CAVV and IAV accept\
            \ different values.\nOn Challenge responses (Stepup, Initiate, Validate)\
            \ this value is currently ignored for CAVV usages in favor of mapping\
            \ to the “Credential Type”. For Enhanced IAV SPA2 usage, if passed, this\
            \ value will override the mapping to the “Credential Type”.\nNote: This\
            \ value will be converted to the Hexadecimal equivalent. Refer to the\
            \ “VCAS Enhanced Authentication Value Support Guide” for details on supported\
            \ authentication values.\n"
        OOBAppURL:
          type: string
          maxLength: 2048
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Universal app link to an authentication app used in OOB authentication. The App URL will open the appropriate location within the authentication app. The issuer is required on Mastercard application-based transactions per Mastercard Bridging Extension Requirements for 2.2 transactions. Max. 2048 characters.
        Credentials:
          type: array
          items:
            $ref: '#/components/schemas/CredentialStepup'
        Reason:
          $ref: '#/components/schemas/Reason'
        Error:
          $ref: '#/components/schemas/ErrorMessage'
        WhyInfo:
          $ref: '#/components/schemas/WhyInfo'
    InitiateActionRequest:
      required:
      - Credentials
      - IssuerId
      - MessageVersion
      - ProcessorId
      - StepupCounter
      - StepupRequestId
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab654
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS generated transaction reference id. Max. 36 characters.
            GUID format.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        CardBrandName:
            $ref: '#/components/schemas/CardBrandName'
        DSTransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Directory Server (DS) generated transaction reference id. GUID
            format.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f258
        3RIIndicator:
            $ref: '#/components/schemas/3RIIndicator'
        ThreeDSRequestorAuthenticationInd:
          $ref: '#/components/schemas/ThreeDSRequestorAuthenticationInd'
        StepupRequestId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Unique identifier to identify the particular Stepup request.
            Length is 36 characters.
          example: 878f4751-4140-4881-9e4a-003e83524f22
        StepupType:
          $ref: '#/components/schemas/StepupType'
        StepupCounter:
          type: integer
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Counter for tracking the number of Stepups. Each stepup can
            go from Stepup -> InitiateAction -> Validate. If the user requests a "resend"
            via the browser templates it will initiate another Stepup request.
        OtpReferenceCode:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: This is a unique value generated with each new OTP value or
            “VerificationToken”. In the instance where the consumer can receive multiple
            OTPs for the same transaction, this can be leveraged to show the consumer
            which OTP the system is expecting to be entered. This value should be
            sent in the SMS or Email along with the VerificationToken and then displayed
            on the consumer screen.
        OOBPushCallBackUrl:
          type: string
          maxLength: 62
          minLength: 62
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: URL provided by VCAS to be used by issuer to return the OOBCallBackValidateStatus response back to the RDX/VCAS service during the OOB CallBack flow. Issuer will append the transaction status to the URL.
        VerificationToken:
          type: string
          maxLength: 18
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Token to be sent to cardholder.
        MessageVersion:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Version of the message based on 3DS spec. Examples - 2.1.0,
            2.2.0
          example: 2.2.0
        RDXMessageVersion:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Version of the RDX protocol being used. This will be used to enable new features without breaking current integrations. Example: 2.2.2 and 2.2.3'
          example: 2.2.3
        MessageCategory:
          type: string
          maxLength: 2
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Identifies the category of the message for a specific use case; 01=PA, 02=NPA
          example: '01'
        Credentials:
          type: array
          items:
            $ref: '#/components/schemas/Credential'
        MerchantInfo:
          $ref: '#/components/schemas/MerchantAppRedirectURLInfo'
        PaymentInfo:
          $ref: '#/components/schemas/PaymentInfo'
        TransactionInfo:
          $ref: '#/components/schemas/InitiateActionTransactionInfo'
    InitiateActionResponse:
      required:
      - Credentials
      - IssuerId
      - ProcessorId
      - Status
      - StepupRequestId
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab963
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Max. 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        StepupRequestId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Length - 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f761
        Language:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Language to display the instructions and prompt to the cardholder.
            This value will decide which template is loaded, provided the correct
            template language is available. If not provided, the language is selected
            based on issuer configuration and browser preference e.g. en-US. Max.
            8 characters.
        Status:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ERROR is returned on any interal/technical issues. AUTHENTICATED
            will return a Y back to merchant.
          enum:
          - SUCCESS
          - AUTHENTICATED
          - STEPUP
          - FAILURE
          - FAILWITHFEEDBACK
          - ERROR
          - BLOCKED
          - REJECTED
        TransStatusReason:
          $ref: '#/components/schemas/TransStatusReason'
        RiskIndicator:
          type: string
          maxLength: 3
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: "If returned, this value will be used in the creation of certain\
            \ Authentication Values (AV) for EMV 3DS. \nThe value passed must conform\
            \ to the current AV value the issuer is using. This may also depend on\
            \ the card brand of the transaction. For example, CAVV and IAV accept\
            \ different values.\nOn Challenge responses (Stepup, Initiate, Validate)\
            \ this value is currently ignored for CAVV usages in favor of mapping\
            \ to the “Credential Type”. For Enhanced IAV SPA2 usage, if passed, this\
            \ value will override the mapping to the “Credential Type”.\nNote: This\
            \ value will be converted to the Hexadecimal equivalent. Refer to the\
            \ “VCAS Enhanced Authentication Value Support Guide” for details on supported\
            \ authentication values.\n"
        Credentials:
          type: array
          items:
            $ref: '#/components/schemas/Credential'
        Reason:
          $ref: '#/components/schemas/Reason'
        Error:
          $ref: '#/components/schemas/ErrorMessage'
        WhyInfo:
          $ref: '#/components/schemas/WhyInfo'
    ValidateRequest:
      required:
      - CredentialResponse
      - IssuerId
      - MessageVersion
      - ProcessorId
      - StepupCounter
      - StepupRequestId
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS provided identifier for the partner. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab481
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: VCAS generated transaction reference id. Max. 36 characters.
            GUID format.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        StepupType:
          $ref: '#/components/schemas/StepupType'
        DSTransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Directory Server (DS) generated transaction reference id. GUID
            format.
          example: 521fa021-4791-4579-a3e9-76de87c219c0
        FirstFactorOutcome:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Will provide the outcome of the first authentication. Success,
            Failure, and Retry are the only valid values. The statuses of Pending,
            FailWithFeedback, Blocked, or Rejected do not apply unless the client
            is performing the validation on the first factor and returns them on the
            second factor RDX Validate response.
          enum:
          - SUCCESS
          - FAILURE
          - RETRY
          - PENDING
          - FAILWITHFEEDBACK
          - BLOCKED
          - REJECTED
        StepupRequestId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Unique identifier to identify the particular Stepup request.
            Length - 36 characters.
          example: 878f4751-4140-4881-9e4a-003e83524f22
        StepupCounter:
          type: integer
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Counter for tracking the number of Stepups. Each stepup can
            go from Stepup -> InitiateAction -> Validate. If the user requests a "resend"
            via the browser templates it will initiate another Stepup request.
        MessageVersion:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Version of the message based on 3DS spec. Examples - 2.1.0, 2.2.0
          example: 2.2.0
        RDXMessageVersion:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: 'Version of the RDX protocol being used. This will be used to enable new features without breaking current integrations. Example: 2.2.2 and 2.2.3'
          example: 2.2.3
        BehavioralBiometricsResult:
          $ref: '#/components/schemas/BehavioralBiometricsResult'
        CredentialResponse:
          type: array
          items:
            $ref: '#/components/schemas/CredentialValidate'
    ValidateResponse:
      required:
      - IssuerId
      - ProcessorId
      - Status
      - StepupRequestId
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Max. 24 characters.
          example: 5723ae630063ac1a9c3ab671
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Max. 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        StepupRequestId:
          type: string
          maxLength: 36
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. Length - 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f618
        Language:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Language to display the instructions and prompt to the cardholder.
            This value will decide which template is loaded, provided the correct
            template language is available. If not provided, the language is selected
            based on issuer configuration and browser preference e.g. en-US. Max.
            8 characters.
        CredentialId:
          type: string
          maxLength: 36
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ID value from StepupResponse->Credential. Length - 36 characters.
        Status:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Status of the validation request. RETRY status will alllow
            the customer to re-attempt validation. PENDING status will initiate another
            Validate Request from VCAS to the issuer after 2 seconds and will only
            be used when StepUpResponse ➤ Type is OUTOFBANDOTHER or BIOMETRIC. STEPUP
            can be returned to allow the customer to challenged again. BLOCKED is
            returned when the customer's card has been disabled and content is displayed
            to the user before returning the status back. FAILURE is returned when
            the customer is not authorized and status is immediately returned. ERROR
            is returned when an internal/technical error has occurred.
          enum:
          - SUCCESS
          - RETRY
          - STEPUP
          - PENDING
          - FAILURE
          - FAILWITHFEEDBACK
          - ERROR
          - BLOCKED
          - REJECTED
        TransStatusReason:
          $ref: '#/components/schemas/TransStatusReason'
        RiskIndicator:
          type: string
          maxLength: 3
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: "If returned, this value will be used in the creation of certain\
            \ Authentication Values (AV) for EMV 3DS. \nThe value passed must conform\
            \ to the current AV value the issuer is using. This may also depend on\
            \ the card brand of the transaction. For example, CAVV and IAV accept\
            \ different values.\nOn Challenge responses (Stepup, Initiate, Validate)\
            \ this value is currently ignored for CAVV usages in favor of mapping\
            \ to the “Credential Type”. For Enhanced IAV SPA2 usage, if passed, this\
            \ value will override the mapping to the “Credential Type”.\nNote: This\
            \ value will be converted to the Hexadecimal equivalent. Please see the\
            \ “VCAS Enhanced Authentication Value Support Guide” for more details\
            \ on current AVs available and the corresponding values.\n"
        Reason:
          $ref: '#/components/schemas/Reason'
        Error:
          $ref: '#/components/schemas/ErrorMessage'
        RReqOverrides:
          $ref: '#/components/schemas/RReqOverrides'
    OOBCallBackValidateStatusError-400:
      type: object
      properties:
        OrgUnitId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 622136db4d0bdc0d4567ca12
        TransactionId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 422c0078-8308-496e-99a7-f81d1baa89d8
        ErrorDetails:
          type: string
          maxLength: 3
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: May indicate that the request does not conform to the specified request structure.
          example: Request does not conform to the request structure.
        ErrorCode:
          type: string
          maxLength: 3
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Numeric code associated with the error.
          example: '400'
        ErrorDescription:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Request does not conform to the request structure. 50 characters max.
          example: Bad Validation Request
        Status:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Validation of the transaction status.
          example: Failure
    OOBCallBackValidateStatusError-401:
      type: object
      properties:
        OrgUnitId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 622136db4d0bdc0d4567ca12
        TransactionId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 422c0078-8308-496e-99a7-f81d1baa89d8
        ErrorDetails:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: May indicate a mismatch between the organization’s certificate and the transaction information.
          example: Unauthorized transaction.
        ErrorCode:
          type: string
          maxLength: 3
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Numeric code associated with the error.
          example: '400'
        ErrorDescription:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Unauthorized transaction. 50 characters max.
          example: Unauthorized transaction
        Status:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Validation of the transaction status.
          example: Failure
    OOBCallBackValidateStatusError-404:
      type: object
      properties:
        OrgUnitId:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 622136db4d0bdc0d4567ca12
        TransactionId:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 422c0078-8308-496e-99a7-f81d1baa89d8
        ErrorDetails:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates the transaction was not found.
          example: Transaction Not Found
        ErrorCode:
          type: string
          maxLength: 3
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Numeric code associated with the error.
          example: '400'
        ErrorDescription:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Transaction Not Found. 50 characters max.
          example: Transaction Not Found
        Status:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Validation of the transaction status.
          example: Failure
    OOBCallBackValidateStatusError-500:
      type: object
      properties:
        OrgUnitId:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 622136db4d0bdc0d4567ca12
        TransactionId:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. 24 characters.
          example: 422c0078-8308-496e-99a7-f81d1baa89d8
        ErrorDetails:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Indicates there was a server error while processing the transaction.
          example: Internal server error.
        ErrorCode:
          type: string
          maxLength: 3
          minLength: 1
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Numeric code associated with the error.
          example: '400'
        ErrorDescription:
          type: string
          maxLength: 50
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Internal server error processing the transaction. 50 characters max.
          example: Internal Server Error
        Status:
          type: string
          maxLength: 20
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Validation of the transaction status.
          example: Failure
    OOBCallBackValidateStatus:
      required:
      - IssuerId
      - ProcessorId
      - Status
      - StepupRequestId
      - TransactionId
      type: object
      properties:
        ProcessorId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. 24 characters.
          example: 5723ae630063ac1a9c3ab079
        IssuerId:
          type: string
          maxLength: 24
          minLength: 24
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. 24 characters.
          example: 5723ae630063ac1a9c3ab671
        TransactionId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from  Request. Max. 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f412
        StepupRequestId:
          type: string
          maxLength: 36
          minLength: 36
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Echoed from Request. Length - 36 characters.
          example: 00ec043e-40b5-4ce4-95c2-9e83b644f618
        Language:
          type: string
          maxLength: 8
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Language to display the instructions and prompt to the cardholder.
            This value will determine which template is loaded, provided the correct
            template language is available. If not provided, the language is selected
            based on issuer configuration and browser preference e.g. en-US. Max.
            8 characters.
          example: en-US
        CredentialId:
          type: string
          maxLength: 36
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: ID value from StepupResponse->Credential. Length - 36 characters.
        Status:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Status of the validation request.
          enum:
          - SUCCESS
          - STEPUP
          - FAILURE
          - FAILWITHFEEDBACK
          - ERROR
          - BLOCKED
          - REJECTED
        TransStatusReason:
          type: string
          maxLength: TBD
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: Provides information on why the Transaction Status field has the specified value
        RiskIndicator:
          type: string
          maxLength: 3
          minLength: TBD
          pattern: '^(0[1-9]|[1-9][0-9])$'
          description: "If returned, this value will be used in the creation of certain\
            \ Authentication Values (AV) for EMV 3DS. \nThe value passed must conform\
            \ to the current AV value the issuer is using. This may also depend on\
            \ the card brand of the transaction. For example, CAVV and IAV accept\
            \ different values.\nOn Challenge responses (Stepup, Initiate, Validate)\
            \ this value is currently ignored for CAVV usages in favor of mapping\
            \ to the “Credential Type”. For Enhanced IAV SPA2 usage, if passed, this\
            \ value will override the mapping to the “Credential Type”.\nNote: This\
            \ value will be converted to the Hexadecimal equivalent. Please see the\
            \ “VCAS Enhanced Authentication Value Support Guide” for more details\
            \ on current AVs available and the corresponding values.\n"
        Reason:
          $ref: '#/components/schemas/Reason'
        Error:
          $ref: '#/components/schemas/ErrorMessage'
        RReqOverrides:
          $ref: '#/components/schemas/RReqOverrides'