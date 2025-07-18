# Copyright 2018 Google LLC All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Gather dependencies and build the executable
FROM golang:1.24.4 as builder
WORKDIR /go/src
COPY . agones.dev/agones

WORKDIR /go/src/agones.dev/agones/examples/autoscaler-webhook
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o server .

# Create the final image that will run the webhook server for FleetAutoscaler webhook policy
FROM gcr.io/distroless/static-debian12:nonroot

COPY --from=builder /go/src/agones.dev/agones/examples/autoscaler-webhook/server /
        
USER nonroot:nonroot
ENTRYPOINT ["/server"]
